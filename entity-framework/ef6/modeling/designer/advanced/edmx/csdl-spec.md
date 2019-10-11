---
title: Спецификация языка CSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182600"
---
# <a name="csdl-specification"></a><span data-ttu-id="9c42b-102">Спецификация CSDL</span><span class="sxs-lookup"><span data-stu-id="9c42b-102">CSDL Specification</span></span>
<span data-ttu-id="9c42b-103">Язык CSDL — это язык на основе XML, описывающий сущности, связи и функции, составляющие концептуальную модель управляемого данными приложения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="9c42b-104">Эта концептуальная модель может использоваться Entity Framework или WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="9c42b-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="9c42b-105">Метаданные, описанные в языке CSDL, используются Entity Framework для соотнесения сущностей и связей, определенных в концептуальной модели, к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="9c42b-106">Дополнительные сведения см. в статье [спецификации языка SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) и [спецификации MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="9c42b-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="9c42b-107">Язык CSDL — это реализация EDM Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9c42b-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="9c42b-108">В приложении Entity Framework метаданные концептуальной модели загружаются из CSDL-файла (написанного на языке CSDL) в экземпляр класса System. Data. Metadata. EDM. коллекций EdmItemCollection и доступны с помощью методов в Класс System. Data. Metadata. EDM. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="9c42b-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="9c42b-109">Entity Framework использует метаданные концептуальной модели для преобразования запросов к концептуальной модели в команды, относящиеся к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="9c42b-110">Конструктор EF сохраняет сведения о концептуальной модели в EDMX файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="9c42b-111">Во время сборки конструктор EF использует сведения в EDMX-файле для создания CSDL-файла, необходимого для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="9c42b-112">Версии языка CSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="9c42b-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="9c42b-113">Версия языка CSDL</span><span class="sxs-lookup"><span data-stu-id="9c42b-113">CSDL Version</span></span> | <span data-ttu-id="9c42b-114">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="9c42b-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="9c42b-115">Язык CSDL v1</span><span class="sxs-lookup"><span data-stu-id="9c42b-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="9c42b-116">Язык CSDL v2</span><span class="sxs-lookup"><span data-stu-id="9c42b-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="9c42b-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="9c42b-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="9c42b-118">Элемент Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-118">Association Element (CSDL)</span></span>

<span data-ttu-id="9c42b-119">Элемент **Association** определяет связь между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="9c42b-120">Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью.</span><span class="sxs-lookup"><span data-stu-id="9c42b-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="9c42b-121">Кратность элемента ассоциации может иметь значение один (1), ноль или один (0.. 1) или многие (\*).</span><span class="sxs-lookup"><span data-stu-id="9c42b-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="9c42b-122">Эти сведения указываются в двух дочерних элементах.</span><span class="sxs-lookup"><span data-stu-id="9c42b-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="9c42b-123">К экземплярам типов сущностей в одном элементе ассоциации можно обращаться посредством свойств навигации или внешних ключей, если они предоставлены в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="9c42b-124">В приложении экземпляр ассоциации представляет конкретную ассоциацию между экземплярами типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="9c42b-125">Экземпляры ассоциации логически сгруппированы в набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="9c42b-126">Элемент **Association** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-127">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-128">Конец (ровно 2 элемента)</span><span class="sxs-lookup"><span data-stu-id="9c42b-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="9c42b-129">ReferentialConstraint (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-130">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-131">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-131">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-132">В следующей таблице описаны атрибуты, которые можно применить к элементу **Association** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="9c42b-133">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-133">Attribute Name</span></span> | <span data-ttu-id="9c42b-134">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-134">Is Required</span></span> | <span data-ttu-id="9c42b-135">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="9c42b-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-136">**Name**</span></span>       | <span data-ttu-id="9c42b-137">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-137">Yes</span></span>         | <span data-ttu-id="9c42b-138">Имя ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-139">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="9c42b-140">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-141">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-142">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-142">Example</span></span>

<span data-ttu-id="9c42b-143">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** , когда внешние ключи не были представлены в типах сущностей **Customer** и **Order** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="9c42b-144">Значения **кратности** для каждого **элемента** ассоциации указывают на то, что многие **заказы** могут быть связаны с **клиентом**, но только один **клиент** может быть связан с **заказом**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="9c42b-145">Кроме того, элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и загруженные в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="9c42b-146">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** , когда внешние ключи были представлены в типах сущностей **Customer** и **Order** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="9c42b-147">При предоставлении внешних ключей связь между сущностями управляется с помощью элемента **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="9c42b-148">Соответствующий элемент AssociationSetMapping не требуется для сопоставления этой ассоциации с источником данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="9c42b-149">Элемент AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="9c42b-150">Элемент **AssociationSet** в языке CSDL — это логический контейнер для экземпляров ассоциаций того же типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="9c42b-151">Набор ассоциаций предоставляет определение группы экземпляров ассоциаций, чтобы их можно было сопоставить с источником данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="9c42b-152">Элемент **AssociationSet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-153">Документация (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-154">End (требуется ровно два элемента)</span><span class="sxs-lookup"><span data-stu-id="9c42b-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="9c42b-155">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="9c42b-156">Атрибут **Association** указывает тип ассоциации, который содержит набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="9c42b-157">Наборы сущностей, которые составляют конечные точки набора ассоциаций, задаются ровно с двумя дочерними **элементами.**</span><span class="sxs-lookup"><span data-stu-id="9c42b-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-158">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-158">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-159">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="9c42b-160">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-160">Attribute Name</span></span>  | <span data-ttu-id="9c42b-161">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-161">Is Required</span></span> | <span data-ttu-id="9c42b-162">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-163">**Name**</span></span>        | <span data-ttu-id="9c42b-164">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-164">Yes</span></span>         | <span data-ttu-id="9c42b-165">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-165">The name of the entity set.</span></span> <span data-ttu-id="9c42b-166">Значение атрибута **Name** не может совпадать со значением атрибута **Association** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="9c42b-167">**Связке**</span><span class="sxs-lookup"><span data-stu-id="9c42b-167">**Association**</span></span> | <span data-ttu-id="9c42b-168">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-168">Yes</span></span>         | <span data-ttu-id="9c42b-169">Полное имя ассоциации, экземпляры которой содержатся в наборе ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="9c42b-170">Ассоциация должна находиться в том же пространстве имен, что и набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-171">К элементу **AssociationSet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="9c42b-172">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-173">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-174">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-174">Example</span></span>

<span data-ttu-id="9c42b-175">В следующем примере показан элемент **EntityContainer** с двумя элементами **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="9c42b-176">Элемент CollectionType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-177">Элемент **CollectionType** в языке CSDL указывает, что параметр функции или тип возвращаемого значения функции является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="9c42b-178">Элемент **CollectionType** может быть дочерним по отношению к элементу Parameter или ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="9c42b-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="9c42b-179">Тип коллекции можно указать с помощью либо атрибута **Type** , либо одного из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="9c42b-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="9c42b-180">**Тип**</span><span class="sxs-lookup"><span data-stu-id="9c42b-180">**CollectionType**</span></span>
-   <span data-ttu-id="9c42b-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="9c42b-181">ReferenceType</span></span>
-   <span data-ttu-id="9c42b-182">RowType</span><span class="sxs-lookup"><span data-stu-id="9c42b-182">RowType</span></span>
-   <span data-ttu-id="9c42b-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="9c42b-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-184">Модель не будет проверять, указан ли тип коллекции с атрибутом **Type** и дочерним элементом.</span><span class="sxs-lookup"><span data-stu-id="9c42b-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-185">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-185">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-186">В следующей таблице описаны атрибуты, которые можно применить к элементу **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="9c42b-187">Обратите внимание, что атрибуты **DefaultValue**, **MaxLength**, **FixedLength**, **точность**, **масштаб**, **Юникод**и **Параметры сортировки** применимы только к коллекциям **едмсимплетипес**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="9c42b-188">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-188">Attribute Name</span></span>                                                          | <span data-ttu-id="9c42b-189">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-189">Is Required</span></span> | <span data-ttu-id="9c42b-190">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-191">**Type**</span></span>                                                                | <span data-ttu-id="9c42b-192">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-192">No</span></span>          | <span data-ttu-id="9c42b-193">Тип коллекции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="9c42b-194">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-194">**Nullable**</span></span>                                                            | <span data-ttu-id="9c42b-195">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-195">No</span></span>          | <span data-ttu-id="9c42b-196">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c42b-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="9c42b-197">> В CSDL v1 свойство сложного типа должно иметь `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="9c42b-198">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="9c42b-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="9c42b-199">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-199">No</span></span>          | <span data-ttu-id="9c42b-200">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c42b-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="9c42b-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="9c42b-202">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-202">No</span></span>          | <span data-ttu-id="9c42b-203">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="9c42b-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="9c42b-205">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-205">No</span></span>          | <span data-ttu-id="9c42b-206">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9c42b-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="9c42b-207">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-207">**Precision**</span></span>                                                           | <span data-ttu-id="9c42b-208">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-208">No</span></span>          | <span data-ttu-id="9c42b-209">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="9c42b-210">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-210">**Scale**</span></span>                                                               | <span data-ttu-id="9c42b-211">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-211">No</span></span>          | <span data-ttu-id="9c42b-212">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-213">**SRID**</span></span>                                                                | <span data-ttu-id="9c42b-214">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-214">No</span></span>          | <span data-ttu-id="9c42b-215">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-216">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="9c42b-217">   Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="9c42b-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="9c42b-218">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-218">**Unicode**</span></span>                                                             | <span data-ttu-id="9c42b-219">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-219">No</span></span>          | <span data-ttu-id="9c42b-220">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="9c42b-221">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-221">**Collation**</span></span>                                                           | <span data-ttu-id="9c42b-222">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-222">No</span></span>          | <span data-ttu-id="9c42b-223">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="9c42b-224">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="9c42b-225">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-226">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-227">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-227">Example</span></span>

<span data-ttu-id="9c42b-228">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию типов сущностей **Person** (как указано с атрибутом **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="9c42b-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="9c42b-229">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="9c42b-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="9c42b-230">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция принимает в качестве параметра коллекцию типов сущностей **отдела** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="9c42b-231">Элемент ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-232">Элемент **complexType** определяет структуру данных, состоящую из свойств **EDMSimpleType** или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="9c42b-233">  Сложный тип может быть свойством типа сущности или другого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="9c42b-234">Сложный тип аналогичен типу сущности, поскольку также определяет данные.</span><span class="sxs-lookup"><span data-stu-id="9c42b-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="9c42b-235">Однако между сложными типами и типами сущности существуют некоторые ключевые различия.</span><span class="sxs-lookup"><span data-stu-id="9c42b-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="9c42b-236">Сложные типы не имеют идентификаторов (или ключей) и поэтому не могут существовать независимо.</span><span class="sxs-lookup"><span data-stu-id="9c42b-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="9c42b-237">Сложные типы могут существовать только как свойства типов сущностей или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="9c42b-238">Сложные типы не могут участвовать в связях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="9c42b-239">Ни один из окончаний ассоциации не может быть сложным типом, поэтому свойства навигации не могут быть определены для сложных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="9c42b-240">Свойство сложного типа не может иметь значение null, хотя каждое скалярное свойство сложного типа может быть установлено в это значение.</span><span class="sxs-lookup"><span data-stu-id="9c42b-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="9c42b-241">Элемент **complexType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-242">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-243">Свойство (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="9c42b-244">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="9c42b-245">В следующей таблице описаны атрибуты, которые можно применить к элементу **complexType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="9c42b-246">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="9c42b-247">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-247">Is Required</span></span> | <span data-ttu-id="9c42b-248">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-249">name</span><span class="sxs-lookup"><span data-stu-id="9c42b-249">Name</span></span>                                                                                                           | <span data-ttu-id="9c42b-250">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-250">Yes</span></span>         | <span data-ttu-id="9c42b-251">Имя сложного типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-251">The name of the complex type.</span></span> <span data-ttu-id="9c42b-252">Имя сложного типа не может совпадать с именем другого сложного типа, типа сущности или сопоставления, которые находятся в области модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="9c42b-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="9c42b-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="9c42b-254">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-254">No</span></span>          | <span data-ttu-id="9c42b-255">Имя другого сложного типа, который является базовым типом определяемого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="9c42b-256">> Этот атрибут неприменим в CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="9c42b-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="9c42b-257">В этой версии не поддерживается наследование для сложных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="9c42b-258">Абстрактный</span><span class="sxs-lookup"><span data-stu-id="9c42b-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="9c42b-259">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-259">No</span></span>          | <span data-ttu-id="9c42b-260">**True** или **false** (значение по умолчанию) в зависимости от того, является ли сложный тип абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="9c42b-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="9c42b-261">> Этот атрибут неприменим в CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="9c42b-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="9c42b-262">Сложные типы в этой версии не могут быть абстрактными типами.</span><span class="sxs-lookup"><span data-stu-id="9c42b-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="9c42b-263">К элементу **complexType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="9c42b-264">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-265">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-266">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-266">Example</span></span>

<span data-ttu-id="9c42b-267">В следующем примере показан сложный тип **Address**с **EDMSimpleType** свойствами **streetAddress**, **City**, **StateOrProvince**, **Country**и **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="9c42b-268">Чтобы определить **адрес** сложного типа (выше) в качестве свойства типа сущности, необходимо объявить тип свойства в определении типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="9c42b-269">В следующем примере показано свойство **Address** в виде сложного типа для типа сущности (**Издатель**):</span><span class="sxs-lookup"><span data-stu-id="9c42b-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="9c42b-270">Элемент DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="9c42b-271">Элемент **DefiningExpression** в языке CSDL содержит выражение Entity SQL, определяющее функцию в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="9c42b-272">В целях проверки элемент **DefiningExpression** может содержать произвольное содержимое.</span><span class="sxs-lookup"><span data-stu-id="9c42b-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="9c42b-273">Однако Entity Framework выдаст исключение во время выполнения, если элемент **DefiningExpression** не содержит допустимых Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-274">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-274">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-275">К элементу **DefiningExpression** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="9c42b-276">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-277">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-278">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-278">Example</span></span>

<span data-ttu-id="9c42b-279">В следующем примере используется элемент **DefiningExpression** для определения функции, возвращающей число лет с момента публикации книги.</span><span class="sxs-lookup"><span data-stu-id="9c42b-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="9c42b-280">Содержимое элемента **DefiningExpression** записывается на Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="9c42b-281">Элемент Dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="9c42b-282">**Зависимый** элемент в языке CSDL является дочерним элементом для элемента ReferentialConstraint и определяет зависимый конец ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="9c42b-283">Элемент **ReferentialConstraint** определяет функциональность, похожую на ограничение ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="9c42b-284">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="9c42b-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="9c42b-285">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="9c42b-286">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="9c42b-287">Элементы **PropertyRef** используются для указания ключей, которые ссылаются на основной элемент.</span><span class="sxs-lookup"><span data-stu-id="9c42b-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="9c42b-288">**Зависимый** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-289">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="9c42b-290">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-291">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-291">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-292">В следующей таблице описаны атрибуты, которые можно применить к **зависимому** элементу.</span><span class="sxs-lookup"><span data-stu-id="9c42b-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="9c42b-293">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-293">Attribute Name</span></span> | <span data-ttu-id="9c42b-294">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-294">Is Required</span></span> | <span data-ttu-id="9c42b-295">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="9c42b-296">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9c42b-296">**Role**</span></span>       | <span data-ttu-id="9c42b-297">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-297">Yes</span></span>         | <span data-ttu-id="9c42b-298">Имя типа сущности в зависимом элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-299">К **зависимому** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="9c42b-300">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-301">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-302">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-302">Example</span></span>

<span data-ttu-id="9c42b-303">В следующем примере показан элемент **ReferentialConstraint** , используемый как часть определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="9c42b-304">Свойство **PublisherID** типа сущности **Book** образует зависимый конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="9c42b-305">Элемент Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="9c42b-306">Элемент **Documentation** в языке CSDL может использоваться для предоставления сведений об объекте, определенном в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="9c42b-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="9c42b-307">В EDMX-файле, если элемент **Documentation** является дочерним элементом элемента, который отображается как объект в области конструктора EF Designer (например, сущность, Ассоциация или свойство), содержимое элемента **Documentation** появится в элементе Окно **свойств** Visual Studio для объекта.</span><span class="sxs-lookup"><span data-stu-id="9c42b-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="9c42b-308">Элемент **Documentation** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-309">**Сводка**: Краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="9c42b-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="9c42b-310">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-310">(zero or one element)</span></span>
-   <span data-ttu-id="9c42b-311">**Лонгдескриптион**: Расширенное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="9c42b-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="9c42b-312">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-312">(zero or one element)</span></span>
-   <span data-ttu-id="9c42b-313">Элементы аннотации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-313">Annotation elements.</span></span> <span data-ttu-id="9c42b-314">(ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-315">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-315">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-316">К элементу **Documentation** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="9c42b-317">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-318">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-319">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-319">Example</span></span>

<span data-ttu-id="9c42b-320">В следующем примере показан элемент **Documentation** в качестве дочернего элемента элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="9c42b-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="9c42b-321">Если приведенный ниже фрагмент кода был в CSDL-файле EDMX-файла, содержимое элементов **Summary** и **лонгдескриптион** будет отображаться в окне **свойств** Visual Studio при щелчке типа сущности `Customer`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="9c42b-322">Элемент End (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-322">End Element (CSDL)</span></span>

<span data-ttu-id="9c42b-323">Элемент **End** в языке CSDL может быть дочерним по отношению к элементу Association или элементу AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="9c42b-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="9c42b-324">В каждом случае роль **конечного** элемента отличается, и применимые атрибуты различаются.</span><span class="sxs-lookup"><span data-stu-id="9c42b-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="9c42b-325">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="9c42b-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="9c42b-326">Элемент **End** (в качестве дочернего элемента **ассоциации** ) определяет тип сущности на одном конце ассоциации и число экземпляров типа сущности, которые могут существовать в этом конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="9c42b-327">Элементы ассоциации определяются при определении ассоциации; ассоциация должна иметь два элемента.</span><span class="sxs-lookup"><span data-stu-id="9c42b-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="9c42b-328">К экземплярам типов сущности на одном элементе ассоциации можно осуществлять доступ с помощью свойств навигации или внешних ключей при условии, что они были предоставлены в типах сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="9c42b-329">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-330">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-331">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-332">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-333">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-333">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-334">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **Association** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="9c42b-335">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-335">Attribute Name</span></span>   | <span data-ttu-id="9c42b-336">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-336">Is Required</span></span> | <span data-ttu-id="9c42b-337">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-338">**Type**</span></span>         | <span data-ttu-id="9c42b-339">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-339">Yes</span></span>         | <span data-ttu-id="9c42b-340">Имя типа сущности на одном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="9c42b-341">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9c42b-341">**Role**</span></span>         | <span data-ttu-id="9c42b-342">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-342">No</span></span>          | <span data-ttu-id="9c42b-343">Имя для элемента ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-343">A name for the association end.</span></span> <span data-ttu-id="9c42b-344">Если имя не было предоставлено, будет использовано имя типа сущности на элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="9c42b-345">**Количества**</span><span class="sxs-lookup"><span data-stu-id="9c42b-345">**Multiplicity**</span></span> | <span data-ttu-id="9c42b-346">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-346">Yes</span></span>         | <span data-ttu-id="9c42b-347">**1**, **0.1**или **\*** в зависимости от числа экземпляров типа сущности, которые могут находиться в конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="9c42b-348">значение **1** указывает, что в конце ассоциации существует ровно один экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="9c42b-349">**0.. 1** указывает, что в конце ассоциации существует ноль или один экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="9c42b-350">**\*** указывает, что в конце ассоциации существует ноль, один или несколько экземпляров типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-351">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9c42b-352">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-353">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-354">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-354">Example</span></span>

<span data-ttu-id="9c42b-355">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="9c42b-356">Значения **кратности** для каждого **элемента** ассоциации указывают на то, что многие **заказы** могут быть связаны с **клиентом**, но только один **клиент** может быть связан с **заказом**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="9c42b-357">Кроме того, элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и которые были загружены в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="9c42b-358">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9c42b-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="9c42b-359">Элемент **End** указывает один конец набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="9c42b-360">Элемент **AssociationSet** должен содержать два элемента **End** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="9c42b-361">Сведения, содержащиеся в элементе **End** , используются при сопоставлении набора ассоциаций с источником данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="9c42b-362">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-363">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-364">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-365">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="9c42b-366">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-367">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-367">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-368">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="9c42b-369">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-369">Attribute Name</span></span> | <span data-ttu-id="9c42b-370">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-370">Is Required</span></span> | <span data-ttu-id="9c42b-371">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="9c42b-372">**EntitySet**</span></span>  | <span data-ttu-id="9c42b-373">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-373">Yes</span></span>         | <span data-ttu-id="9c42b-374">Имя элемента **EntitySet** , определяющего один конец родительского элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="9c42b-375">Элемент **EntitySet** должен быть определен в том же контейнере сущностей, что и родительский элемент **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="9c42b-376">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9c42b-376">**Role**</span></span>       | <span data-ttu-id="9c42b-377">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-377">No</span></span>          | <span data-ttu-id="9c42b-378">Имя элемента набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-378">The name of the association set end.</span></span> <span data-ttu-id="9c42b-379">Если атрибут **Role** не используется, имя конца набора ассоциаций будет именем набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="9c42b-380">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9c42b-381">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-382">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-383">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-383">Example</span></span>

<span data-ttu-id="9c42b-384">В следующем примере показан элемент **EntityContainer** с двумя элементами **AssociationSet** , каждый из которых содержит два элемента **End** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="9c42b-385">Элемент EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="9c42b-386">Элемент **EntityContainer** в языке CSDL — это логический контейнер для наборов сущностей, наборов ассоциаций и импортов функций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="9c42b-387">Контейнер сущностей концептуальной модели сопоставляется с контейнером сущностей модели хранения через элемент EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="9c42b-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="9c42b-388">Контейнер сущностей режима хранения описывает структуру базы данных: наборы сущностей описывают таблицы, наборы ассоциаций описывают ограничения внешних ключей, функции импорта описывают хранимые процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="9c42b-389">Элемент **EntityContainer** может иметь один или несколько элементов документации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="9c42b-390">Если элемент **Documentation** присутствует, он должен предшествовать всем элементам **EntitySet**, **AssociationSet**и **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="9c42b-391">Элемент **EntityContainer** может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="9c42b-392">EntitySet</span></span>
-   <span data-ttu-id="9c42b-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9c42b-393">AssociationSet</span></span>
-   <span data-ttu-id="9c42b-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="9c42b-394">FunctionImport</span></span>
-   <span data-ttu-id="9c42b-395">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9c42b-395">Annotation elements</span></span>

<span data-ttu-id="9c42b-396">Можно расширить элемент **EntityContainer** , включив в него содержимое другого **контейнера EntityContainer** , который находится в том же пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="9c42b-397">Чтобы включить содержимое другого **контейнера EntityContainer**, в ссылающемся элементе **EntityContainer** установите значение атрибута **extends** в имя элемента **EntityContainer** , который требуется включить.</span><span class="sxs-lookup"><span data-stu-id="9c42b-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="9c42b-398">Все дочерние элементы включаемого элемента **EntityContainer** будут рассматриваться как дочерние элементы ссылающегося элемента **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-399">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-399">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-400">В следующей таблице описаны атрибуты, которые можно применить к элементу **using** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="9c42b-401">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-401">Attribute Name</span></span> | <span data-ttu-id="9c42b-402">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-402">Is Required</span></span> | <span data-ttu-id="9c42b-403">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="9c42b-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-404">**Name**</span></span>       | <span data-ttu-id="9c42b-405">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-405">Yes</span></span>         | <span data-ttu-id="9c42b-406">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="9c42b-407">**Предоставляет**</span><span class="sxs-lookup"><span data-stu-id="9c42b-407">**Extends**</span></span>    | <span data-ttu-id="9c42b-408">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-408">No</span></span>          | <span data-ttu-id="9c42b-409">Имя другого контейнера сущностей в том же пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-410">К элементу **EntityContainer** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="9c42b-411">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-412">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-413">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-413">Example</span></span>

<span data-ttu-id="9c42b-414">В следующем примере показан элемент **EntityContainer** , определяющий три набора сущностей и два набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9c42b-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="9c42b-415">Элемент EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="9c42b-416">Элемент **EntitySet** в языке определения концептуальной схемы — это логический контейнер для экземпляров типа сущности и экземпляров любого типа, производного от этого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="9c42b-417">Связь между типом сущности и набором сущностей аналогична связи между строкой и таблицей в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="9c42b-418">Тип сущности, как и строка, определяет ряд взаимосвязанных данных, а набор сущностей, как и таблица, содержит экземпляры этого определения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="9c42b-419">Набор сущностей предоставляет конструкцию для группирования экземпляров типа сущности, чтобы их можно было сопоставлять со связанными структурами данных в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="9c42b-420">Можно определить больше одного набора сущностей для конкретного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-421">Конструктор EF не поддерживает концептуальные модели, содержащие несколько наборов сущностей для каждого типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="9c42b-422">Элемент **EntitySet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-423">Элемент Documentation (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-424">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-425">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-425">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-426">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="9c42b-427">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-427">Attribute Name</span></span> | <span data-ttu-id="9c42b-428">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-428">Is Required</span></span> | <span data-ttu-id="9c42b-429">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-430">**Name**</span></span>       | <span data-ttu-id="9c42b-431">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-431">Yes</span></span>         | <span data-ttu-id="9c42b-432">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="9c42b-433">**Типе**</span><span class="sxs-lookup"><span data-stu-id="9c42b-433">**EntityType**</span></span> | <span data-ttu-id="9c42b-434">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-434">Yes</span></span>         | <span data-ttu-id="9c42b-435">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="9c42b-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-436">К элементу **EntitySet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="9c42b-437">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-438">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-439">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-439">Example</span></span>

<span data-ttu-id="9c42b-440">В следующем примере показан элемент **EntityContainer** с тремя элементами **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="9c42b-441">Предусмотрена возможность определять несколько наборов сущностей на тип (модель MEST).</span><span class="sxs-lookup"><span data-stu-id="9c42b-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="9c42b-442">В следующем примере определяется контейнер сущностей с двумя наборами сущностей для типа сущности **Book** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="9c42b-443">Элемент EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-444">Элемент **EntityType** представляет структуру концепции верхнего уровня, например Customer или Order, в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="9c42b-445">Тип сущности — это шаблон для экземпляров типов сущностей в приложении.</span><span class="sxs-lookup"><span data-stu-id="9c42b-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="9c42b-446">Каждый шаблон содержит следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="9c42b-447">Уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="9c42b-447">A unique name.</span></span> <span data-ttu-id="9c42b-448">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="9c42b-448">(Required.)</span></span>
-   <span data-ttu-id="9c42b-449">Ключ сущности, определяемый одним или несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="9c42b-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="9c42b-450">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="9c42b-450">(Required.)</span></span>
-   <span data-ttu-id="9c42b-451">Свойства содержащихся данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-451">Properties for containing data.</span></span> <span data-ttu-id="9c42b-452">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="9c42b-452">(Optional.)</span></span>
-   <span data-ttu-id="9c42b-453">Свойства навигации, позволяющие осуществлять переход от одного элемента ассоциации к другому.</span><span class="sxs-lookup"><span data-stu-id="9c42b-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="9c42b-454">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="9c42b-454">(Optional.)</span></span>

<span data-ttu-id="9c42b-455">В приложении экземпляр типа сущности представляет определенный объект (например, определенного клиента или заказ).</span><span class="sxs-lookup"><span data-stu-id="9c42b-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="9c42b-456">Каждый экземпляр типа сущности в наборе сущностей должен иметь уникальный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="9c42b-457">Два экземпляра типа сущности считаются равными, только если они являются экземплярами одного типа и значения их ключей сущности равны.</span><span class="sxs-lookup"><span data-stu-id="9c42b-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="9c42b-458">Элемент **EntityType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-459">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-460">Ключ (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-461">Свойство (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="9c42b-462">Свойство NavigationProperty (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="9c42b-463">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-464">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-464">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-465">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="9c42b-466">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="9c42b-467">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-467">Is Required</span></span> | <span data-ttu-id="9c42b-468">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="9c42b-470">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-470">Yes</span></span>         | <span data-ttu-id="9c42b-471">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="9c42b-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="9c42b-473">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-473">No</span></span>          | <span data-ttu-id="9c42b-474">Имя другого типа сущности, являющегося базовым типом для определяемого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="9c42b-475">**Выделяет**</span><span class="sxs-lookup"><span data-stu-id="9c42b-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="9c42b-476">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-476">No</span></span>          | <span data-ttu-id="9c42b-477">**Значение true** или **false**в зависимости от того, является ли тип сущности абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="9c42b-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="9c42b-478">**Пользу**</span><span class="sxs-lookup"><span data-stu-id="9c42b-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="9c42b-479">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-479">No</span></span>          | <span data-ttu-id="9c42b-480">**Значение true** или **false** в зависимости от того, является ли тип сущности открытым типом сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="9c42b-481">> Атрибут **OpenType** применим только к типам сущностей, определенным в концептуальных моделях, которые используются со службами ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="9c42b-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="9c42b-482">К элементу **EntityType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="9c42b-483">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-484">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-485">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-485">Example</span></span>

<span data-ttu-id="9c42b-486">В следующем примере показан элемент **EntityType** с тремя элементами **свойств** и двумя элементами **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="9c42b-487">Элемент EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-488">Элемент **enumType** представляет перечисляемый тип.</span><span class="sxs-lookup"><span data-stu-id="9c42b-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="9c42b-489">Элемент **enumType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-490">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-491">Элемент (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="9c42b-492">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-493">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-493">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-494">В следующей таблице описаны атрибуты, которые можно применить к элементу **enumType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="9c42b-495">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-495">Attribute Name</span></span>     | <span data-ttu-id="9c42b-496">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-496">Is Required</span></span> | <span data-ttu-id="9c42b-497">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-498">**Name**</span></span>           | <span data-ttu-id="9c42b-499">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-499">Yes</span></span>         | <span data-ttu-id="9c42b-500">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="9c42b-501">**Пометки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-501">**IsFlags**</span></span>        | <span data-ttu-id="9c42b-502">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-502">No</span></span>          | <span data-ttu-id="9c42b-503">**Значение true** или **false**в зависимости от того, можно ли использовать тип перечисления в качестве набора флагов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="9c42b-504">Значение по умолчанию — **false.**</span><span class="sxs-lookup"><span data-stu-id="9c42b-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="9c42b-505">**ундерлингтипе**</span><span class="sxs-lookup"><span data-stu-id="9c42b-505">**UnderlyingType**</span></span> | <span data-ttu-id="9c42b-506">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-506">No</span></span>          | <span data-ttu-id="9c42b-507">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** или **EDM. SByte** , определяющие диапазон значений типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="9c42b-508">  Базовым типом элементов перечисления по умолчанию является **EDM. Int32.**</span><span class="sxs-lookup"><span data-stu-id="9c42b-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-509">К элементу **enumType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="9c42b-510">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-511">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-512">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-512">Example</span></span>

<span data-ttu-id="9c42b-513">В следующем примере показан элемент **enumType** с тремя элементами **member** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="9c42b-514">Элемент Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-514">Function Element (CSDL)</span></span>

<span data-ttu-id="9c42b-515">Элемент **Function** в языке CSDL используется для определения или объявления функций в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="9c42b-516">Функция определяется с помощью элемента DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="9c42b-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="9c42b-517">Элемент **функции** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-518">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-519">Параметр (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="9c42b-520">DefiningExpression (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-521">ReturnType (функция) (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-522">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="9c42b-523">Тип возвращаемого значения для функции должен быть указан либо с помощью элемента **ReturnType** (Function), либо с помощью атрибута **ReturnType** (см. ниже), но не в обоих.</span><span class="sxs-lookup"><span data-stu-id="9c42b-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="9c42b-524">Возвращаемым типом может быть EdmSimpleType, тип сущности, сложный тип, строковый тип, ссылочный тип или коллекция, которая включает один из этих типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-525">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-525">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-526">В следующей таблице описаны атрибуты, которые можно применить к элементу **Function** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="9c42b-527">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-527">Attribute Name</span></span> | <span data-ttu-id="9c42b-528">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-528">Is Required</span></span> | <span data-ttu-id="9c42b-529">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="9c42b-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-530">**Name**</span></span>       | <span data-ttu-id="9c42b-531">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-531">Yes</span></span>         | <span data-ttu-id="9c42b-532">Имя функции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-532">The name of the function.</span></span>          |
| <span data-ttu-id="9c42b-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-533">**ReturnType**</span></span> | <span data-ttu-id="9c42b-534">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-534">No</span></span>          | <span data-ttu-id="9c42b-535">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-536">К элементу **функции** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="9c42b-537">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-538">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-539">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-539">Example</span></span>

<span data-ttu-id="9c42b-540">В следующем примере используется элемент **Function** для определения функции, возвращающей количество лет с момента найма инструктора.</span><span class="sxs-lookup"><span data-stu-id="9c42b-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="9c42b-541">Элемент FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="9c42b-542">Элемент **FunctionImport** в языке CSDL представляет функцию, которая определена в источнике данных, но доступна объектам через концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="9c42b-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="9c42b-543">Например, элемент Function в модели хранения может использоваться для представления хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="9c42b-544">Элемент **FunctionImport** в концептуальной модели представляет соответствующую функцию в Entity Framework приложении и сопоставляется с функцией модели хранения с помощью элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="9c42b-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="9c42b-545">При вызове функции в приложении соответствующая хранимая процедура выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="9c42b-546">Элемент **FunctionImport** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-547">Документация (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-548">Параметр (разрешено ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="9c42b-549">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="9c42b-550">ReturnType (FunctionImport) (разрешено ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="9c42b-551">Для каждого параметра, который принимает функция, должен быть определен один элемент **параметра** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="9c42b-552">Тип возвращаемого значения для функции должен быть указан либо с помощью элемента **ReturnType** (FunctionImport), либо с помощью атрибута **ReturnType** (см. ниже), но не в обоих.</span><span class="sxs-lookup"><span data-stu-id="9c42b-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="9c42b-553">Значение возвращаемого типа должно быть коллекцией EdmSimpleType, EntityType или ComplexType.</span><span class="sxs-lookup"><span data-stu-id="9c42b-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-554">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-554">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-555">В следующей таблице описаны атрибуты, которые можно применить к элементу **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="9c42b-556">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-556">Attribute Name</span></span>   | <span data-ttu-id="9c42b-557">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-557">Is Required</span></span> | <span data-ttu-id="9c42b-558">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-559">**Name**</span></span>         | <span data-ttu-id="9c42b-560">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-560">Yes</span></span>         | <span data-ttu-id="9c42b-561">Имя импортируемой функции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="9c42b-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-562">**ReturnType**</span></span>   | <span data-ttu-id="9c42b-563">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-563">No</span></span>          | <span data-ttu-id="9c42b-564">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-564">The type that the function returns.</span></span> <span data-ttu-id="9c42b-565">Не используйте этот атрибут, если функция не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="9c42b-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="9c42b-566">В противном случае значение должно быть коллекцией типа ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="9c42b-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="9c42b-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="9c42b-567">**EntitySet**</span></span>    | <span data-ttu-id="9c42b-568">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-568">No</span></span>          | <span data-ttu-id="9c42b-569">Если функция возвращает коллекцию типов сущностей, то значение **EntitySet** должно быть набором сущностей, к которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="9c42b-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="9c42b-570">В противном случае не следует использовать атрибут **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="9c42b-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="9c42b-571">**IsComposable**</span></span> | <span data-ttu-id="9c42b-572">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-572">No</span></span>          | <span data-ttu-id="9c42b-573">Если значение равно true, функция является составной (функция с табличным значением) и может использоваться в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="9c42b-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="9c42b-574">  Значение по умолчанию — **false**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="9c42b-575">К элементу **FunctionImport** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="9c42b-576">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-577">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-578">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-578">Example</span></span>

<span data-ttu-id="9c42b-579">В следующем примере показан элемент **FunctionImport** , принимающий один параметр и возвращающий коллекцию типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="9c42b-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="9c42b-580">Элемент Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-580">Key Element (CSDL)</span></span>

<span data-ttu-id="9c42b-581">Элемент **Key** является дочерним элементом элемента EntityType и определяет *ключ сущности* (свойство или набор свойств типа сущности, определяющего удостоверение).</span><span class="sxs-lookup"><span data-stu-id="9c42b-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="9c42b-582">Свойства, составляющие ключ сущности, выбираются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="9c42b-583">Значения свойств ключа сущности должны уникально определять экземпляр типа сущности внутри набора сущностей во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="9c42b-584">Свойства, составляющие ключ сущности, должны гарантировать уникальность экземпляра набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="9c42b-585">Элемент **Key** определяет ключ сущности, ссылаясь на одно или несколько свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="9c42b-586">Элемент **Key** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9c42b-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="9c42b-587">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="9c42b-588">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-589">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-589">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-590">К элементу **Key** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="9c42b-591">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-592">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-593">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-593">Example</span></span>

<span data-ttu-id="9c42b-594">В приведенном ниже примере определяется тип сущности « **Book**».</span><span class="sxs-lookup"><span data-stu-id="9c42b-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="9c42b-595">Ключ сущности определяется с помощью ссылки на свойство **ISBN** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="9c42b-596">Свойство **ISBN** является хорошим выбором для ключа сущности, так как Международный номер книги (ISBN) является уникальным идентификатором книги.</span><span class="sxs-lookup"><span data-stu-id="9c42b-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="9c42b-597">В следующем примере показан тип сущности (**Author**), имеющий ключ сущности, состоящий из двух свойств: **Name** и **Address**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="9c42b-598">Использование **имени** и **адреса** для ключа сущности является разумным выбором, поскольку два автора одного и того же имени вряд ли будут находиться на одном и том же адресе.</span><span class="sxs-lookup"><span data-stu-id="9c42b-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="9c42b-599">Однако такой выбор ключа сущности не гарантирует уникальность ключей сущности в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="9c42b-600">В этом случае рекомендуется добавить свойство, например **аусорид**, которое может быть использовано для уникальной идентификации автора.</span><span class="sxs-lookup"><span data-stu-id="9c42b-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="9c42b-601">Элемент Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-601">Member Element (CSDL)</span></span>

<span data-ttu-id="9c42b-602">Элемент **member** является дочерним элементом элемента enumType и определяет элемент перечисляемого типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-603">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-603">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-604">В следующей таблице описаны атрибуты, которые можно применить к элементу **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="9c42b-605">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-605">Attribute Name</span></span> | <span data-ttu-id="9c42b-606">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-606">Is Required</span></span> | <span data-ttu-id="9c42b-607">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-608">**Name**</span></span>       | <span data-ttu-id="9c42b-609">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-609">Yes</span></span>         | <span data-ttu-id="9c42b-610">Имя элемента.</span><span class="sxs-lookup"><span data-stu-id="9c42b-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="9c42b-611">**Значение**</span><span class="sxs-lookup"><span data-stu-id="9c42b-611">**Value**</span></span>      | <span data-ttu-id="9c42b-612">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-612">No</span></span>          | <span data-ttu-id="9c42b-613">Значение элемента.</span><span class="sxs-lookup"><span data-stu-id="9c42b-613">The value of the member.</span></span> <span data-ttu-id="9c42b-614">По умолчанию первый элемент имеет значение 0, а значение каждого последующего перечислителя увеличивается на 1.</span><span class="sxs-lookup"><span data-stu-id="9c42b-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="9c42b-615">Могут существовать несколько членов с одинаковыми значениями.</span><span class="sxs-lookup"><span data-stu-id="9c42b-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-616">К элементу **FunctionImport** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="9c42b-617">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-618">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-619">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-619">Example</span></span>

<span data-ttu-id="9c42b-620">В следующем примере показан элемент **enumType** с тремя элементами **member** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="9c42b-621">Элемент NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="9c42b-622">Элемент **NavigationProperty** определяет свойство навигации, которое предоставляет ссылку на другой конец ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="9c42b-623">В отличие от свойств, определенных с помощью элемента Property, свойства навигации не определяют форму и характеристики данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="9c42b-624">Они предоставляют возможность навигации по ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="9c42b-625">Обратите внимание, что свойства навигации являются необязательными для обоих типов сущностей, расположенных в конечных элементах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="9c42b-626">Если свойство навигации было определено для типа сущности на одном конечном элементе ассоциации, то определять его для типа сущности на другом конечном элементе необязательно.</span><span class="sxs-lookup"><span data-stu-id="9c42b-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="9c42b-627">Тип данных, возвращаемый свойством навигации, определяется кратностью в удаленном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="9c42b-628">Например, предположим, что свойство навигации **ордерснавпроп**существует в типе сущности **Customer** и выполняет навигацию между **клиентом** и **заказом**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="9c42b-629">Так как удаленная ассоциация для свойства навигации имеет кратность many (\*), ее тип данных — это коллекция (в **порядке**).</span><span class="sxs-lookup"><span data-stu-id="9c42b-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="9c42b-630">Аналогично, если свойство навигации **кустомернавпроп**существует в типе сущности **Order** , его тип данных — **Customer** , так как кратность удаленного элемента равна единице (1).</span><span class="sxs-lookup"><span data-stu-id="9c42b-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="9c42b-631">Элемент **NavigationProperty** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-632">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-633">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-634">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-634">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-635">В следующей таблице описаны атрибуты, которые можно применить к элементу **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="9c42b-636">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-636">Attribute Name</span></span>   | <span data-ttu-id="9c42b-637">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-637">Is Required</span></span> | <span data-ttu-id="9c42b-638">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-639">**Name**</span></span>         | <span data-ttu-id="9c42b-640">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-640">Yes</span></span>         | <span data-ttu-id="9c42b-641">Имя свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="9c42b-642">**Потом**</span><span class="sxs-lookup"><span data-stu-id="9c42b-642">**Relationship**</span></span> | <span data-ttu-id="9c42b-643">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-643">Yes</span></span>         | <span data-ttu-id="9c42b-644">Имя ассоциации, расположенной в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="9c42b-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="9c42b-645">**ToRole**</span></span>       | <span data-ttu-id="9c42b-646">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-646">Yes</span></span>         | <span data-ttu-id="9c42b-647">Конечная точка ассоциации на которой заканчивается навигация.</span><span class="sxs-lookup"><span data-stu-id="9c42b-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="9c42b-648">Значение атрибута **ToRole** должно совпадать со значением одного из атрибутов **роли** , определенных на одной из сторон ассоциации (определена в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="9c42b-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="9c42b-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="9c42b-649">**FromRole**</span></span>     | <span data-ttu-id="9c42b-650">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-650">Yes</span></span>         | <span data-ttu-id="9c42b-651">Конечная точка ассоциации с которой начинается навигация.</span><span class="sxs-lookup"><span data-stu-id="9c42b-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="9c42b-652">Значение атрибута **FromRole** должно совпадать со значением одного из атрибутов **роли** , определенных на одной из сторон ассоциации (определена в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="9c42b-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-653">К элементу **NavigationProperty** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="9c42b-654">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-655">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-656">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-656">Example</span></span>

<span data-ttu-id="9c42b-657">В следующем примере определяется тип сущности (**Book**) с двумя свойствами навигации (**публишедби** и **вриттенби**):</span><span class="sxs-lookup"><span data-stu-id="9c42b-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="9c42b-658">Элемент OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="9c42b-659">Элемент **OnDelete** в языке CSDL определяет поведение, связанное с Ассоциацией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="9c42b-660">Если атрибут **Action** имеет значение **CASCADE** на одном конце ассоциации, то связанные типы сущностей на другом конце ассоциации удаляются при удалении типа сущности в первом конце.</span><span class="sxs-lookup"><span data-stu-id="9c42b-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="9c42b-661">Если связь между двумя типами сущностей является связью первичного ключа, то загруженный зависимый объект удаляется, когда объект Principal на другом конце ассоциации удаляется независимо от спецификации **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="9c42b-662">Элемент **OnDelete** влияет только на поведение среды выполнения приложения. Он не влияет на поведение в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="9c42b-663">Поведение, определенное в источнике данных, должно совпадать с поведением, определенным для приложения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="9c42b-664">Элемент **OnDelete** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-665">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-666">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-667">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-667">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-668">В следующей таблице описаны атрибуты, которые можно применить к элементу **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="9c42b-669">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-669">Attribute Name</span></span> | <span data-ttu-id="9c42b-670">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-670">Is Required</span></span> | <span data-ttu-id="9c42b-671">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-672">**Действие**</span><span class="sxs-lookup"><span data-stu-id="9c42b-672">**Action**</span></span>     | <span data-ttu-id="9c42b-673">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-673">Yes</span></span>         | <span data-ttu-id="9c42b-674">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-674">**Cascade** or **None**.</span></span> <span data-ttu-id="9c42b-675">Если параметр **CASCADE**, зависимые типы сущностей будут удалены при удалении основного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="9c42b-676">Если **нет**, зависимые типы сущностей не будут удаляться при удалении типа основной сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-677">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="9c42b-678">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-679">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-680">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-680">Example</span></span>

<span data-ttu-id="9c42b-681">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="9c42b-682">Элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и загруженные в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="9c42b-683">Элемент Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="9c42b-684">Элемент **Parameter** в языке CSDL может быть дочерним по отношению к элементу FunctionImport или Function.</span><span class="sxs-lookup"><span data-stu-id="9c42b-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="9c42b-685">Приложение, использующее элемент FunctionImport</span><span class="sxs-lookup"><span data-stu-id="9c42b-685">FunctionImport Element Application</span></span>

<span data-ttu-id="9c42b-686">Элемент **Parameter** (как дочерний элемент элемента **FunctionImport** ) используется для определения входных и выходных параметров для импортов функций, объявленных в языке CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="9c42b-687">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-688">Документация (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-689">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-690">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-690">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-691">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="9c42b-692">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-692">Attribute Name</span></span> | <span data-ttu-id="9c42b-693">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-693">Is Required</span></span> | <span data-ttu-id="9c42b-694">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-695">**Name**</span></span>       | <span data-ttu-id="9c42b-696">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-696">Yes</span></span>         | <span data-ttu-id="9c42b-697">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="9c42b-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-698">**Type**</span></span>       | <span data-ttu-id="9c42b-699">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-699">Yes</span></span>         | <span data-ttu-id="9c42b-700">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-700">The parameter type.</span></span> <span data-ttu-id="9c42b-701">Значение должно быть **EDMSimpleType** или сложным типом, который находится в области действия модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="9c42b-702">**Режим**</span><span class="sxs-lookup"><span data-stu-id="9c42b-702">**Mode**</span></span>       | <span data-ttu-id="9c42b-703">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-703">No</span></span>          | <span data-ttu-id="9c42b-704">**В**, **out**или **InOut** в зависимости от того, является ли параметр входным, выходным или входным или выходным.</span><span class="sxs-lookup"><span data-stu-id="9c42b-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="9c42b-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-705">**MaxLength**</span></span>  | <span data-ttu-id="9c42b-706">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-706">No</span></span>          | <span data-ttu-id="9c42b-707">Максимально допустимая длина параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="9c42b-708">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-708">**Precision**</span></span>  | <span data-ttu-id="9c42b-709">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-709">No</span></span>          | <span data-ttu-id="9c42b-710">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-711">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-711">**Scale**</span></span>      | <span data-ttu-id="9c42b-712">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-712">No</span></span>          | <span data-ttu-id="9c42b-713">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="9c42b-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-714">**SRID**</span></span>       | <span data-ttu-id="9c42b-715">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-715">No</span></span>          | <span data-ttu-id="9c42b-716">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-717">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="9c42b-718">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-719">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="9c42b-720">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-721">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-722">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-722">Example</span></span>

<span data-ttu-id="9c42b-723">В следующем примере показан элемент **FunctionImport** с одним дочерним элементом **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="9c42b-724">Функция принимает один входной параметр и возвращает коллекцию типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="9c42b-725">Приложение, использующее элемент Function</span><span class="sxs-lookup"><span data-stu-id="9c42b-725">Function Element Application</span></span>

<span data-ttu-id="9c42b-726">Элемент **Parameter** (в качестве дочернего элемента **функции** ) определяет параметры для функций, определенных или объявленных в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="9c42b-727">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-728">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="9c42b-729">Тип CollectionType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="9c42b-730">ReferenceType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="9c42b-731">RowType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-732">Только один из элементов **CollectionType**, **ReferenceType**или **RowType** может быть дочерним элементом элемента **Property** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="9c42b-733">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-734">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="9c42b-735">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-736">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-736">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-737">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="9c42b-738">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-738">Attribute Name</span></span>   | <span data-ttu-id="9c42b-739">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-739">Is Required</span></span> | <span data-ttu-id="9c42b-740">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-741">**Name**</span></span>         | <span data-ttu-id="9c42b-742">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-742">Yes</span></span>         | <span data-ttu-id="9c42b-743">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="9c42b-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-744">**Type**</span></span>         | <span data-ttu-id="9c42b-745">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-745">No</span></span>          | <span data-ttu-id="9c42b-746">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="9c42b-746">The parameter type.</span></span> <span data-ttu-id="9c42b-747">Параметр может иметь любой из следующих типов (или быть коллекцией этих типов):</span><span class="sxs-lookup"><span data-stu-id="9c42b-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="9c42b-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="9c42b-749">тип сущности</span><span class="sxs-lookup"><span data-stu-id="9c42b-749">entity type</span></span> <br/> <span data-ttu-id="9c42b-750">сложный тип</span><span class="sxs-lookup"><span data-stu-id="9c42b-750">complex type</span></span> <br/> <span data-ttu-id="9c42b-751">тип строки</span><span class="sxs-lookup"><span data-stu-id="9c42b-751">row type</span></span> <br/> <span data-ttu-id="9c42b-752">ссылочный тип</span><span class="sxs-lookup"><span data-stu-id="9c42b-752">reference type</span></span>                             |
| <span data-ttu-id="9c42b-753">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-753">**Nullable**</span></span>     | <span data-ttu-id="9c42b-754">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-754">No</span></span>          | <span data-ttu-id="9c42b-755">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли свойство иметь значение **null** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="9c42b-756">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="9c42b-756">**DefaultValue**</span></span> | <span data-ttu-id="9c42b-757">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-757">No</span></span>          | <span data-ttu-id="9c42b-758">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c42b-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="9c42b-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-759">**MaxLength**</span></span>    | <span data-ttu-id="9c42b-760">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-760">No</span></span>          | <span data-ttu-id="9c42b-761">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-762">**FixedLength**</span></span>  | <span data-ttu-id="9c42b-763">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-763">No</span></span>          | <span data-ttu-id="9c42b-764">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9c42b-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="9c42b-765">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-765">**Precision**</span></span>    | <span data-ttu-id="9c42b-766">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-766">No</span></span>          | <span data-ttu-id="9c42b-767">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9c42b-768">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-768">**Scale**</span></span>        | <span data-ttu-id="9c42b-769">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-769">No</span></span>          | <span data-ttu-id="9c42b-770">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="9c42b-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-771">**SRID**</span></span>         | <span data-ttu-id="9c42b-772">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-772">No</span></span>          | <span data-ttu-id="9c42b-773">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-774">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9c42b-775">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9c42b-776">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-776">**Unicode**</span></span>      | <span data-ttu-id="9c42b-777">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-777">No</span></span>          | <span data-ttu-id="9c42b-778">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="9c42b-779">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-779">**Collation**</span></span>    | <span data-ttu-id="9c42b-780">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-780">No</span></span>          | <span data-ttu-id="9c42b-781">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="9c42b-782">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="9c42b-783">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-784">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-785">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-785">Example</span></span>

<span data-ttu-id="9c42b-786">В следующем примере показан элемент **Function** , использующий один дочерний элемент **Parameter** для определения параметра функции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="9c42b-787">Элемент Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="9c42b-788">Элемент **Principal** в языке CSDL является дочерним элементом для элемента ReferentialConstraint, который определяет основной элемент справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="9c42b-789">Элемент **ReferentialConstraint** определяет функциональность, похожую на ограничение ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="9c42b-790">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="9c42b-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="9c42b-791">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="9c42b-792">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="9c42b-793">Элементы **PropertyRef** используются для указания ключей, на которые ссылается зависимый элемент.</span><span class="sxs-lookup"><span data-stu-id="9c42b-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="9c42b-794">Элемент **Principal** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-795">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="9c42b-796">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-797">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-797">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-798">В следующей таблице описаны атрибуты, которые можно применить к элементу **Principal** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="9c42b-799">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-799">Attribute Name</span></span> | <span data-ttu-id="9c42b-800">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-800">Is Required</span></span> | <span data-ttu-id="9c42b-801">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="9c42b-802">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9c42b-802">**Role**</span></span>       | <span data-ttu-id="9c42b-803">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-803">Yes</span></span>         | <span data-ttu-id="9c42b-804">Имя типа сущности в основном конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-805">К **основному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="9c42b-806">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-807">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-808">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-808">Example</span></span>

<span data-ttu-id="9c42b-809">В следующем примере показан элемент **ReferentialConstraint** , который является частью определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="9c42b-810">Свойство **ID** типа сущности **издателя** образует основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="9c42b-811">Элемент Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-811">Property Element (CSDL)</span></span>

<span data-ttu-id="9c42b-812">Элемент **Property** в языке CSDL может быть дочерним по отношению к элементу EntityType, элементу complexType или элементу RowType.</span><span class="sxs-lookup"><span data-stu-id="9c42b-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="9c42b-813">Применение элементов EntityType и ComplexType</span><span class="sxs-lookup"><span data-stu-id="9c42b-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="9c42b-814">Элементы **свойств** (как дочерние элементы **EntityType** или **complexType** ) определяют форму и характеристики данных, которые будут содержаться в экземпляре типа сущности или экземпляра сложного типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="9c42b-815">Свойства в концептуальной модели аналогичны свойствам, которые определены в классе.</span><span class="sxs-lookup"><span data-stu-id="9c42b-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="9c42b-816">По такому же принципу, как свойства, относящиеся к классу, определяют форму класса и несут информацию об объектах, свойства в концептуальной модели определяют форму типа сущности и несут информацию об экземплярах типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="9c42b-817">Элемент **Property** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-818">Элемент Documentation (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-819">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="9c42b-820">К элементу **Свойства** можно применить следующие аспекты: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **точность**, **масштаб**, **Юникод**, **Параметры сортировки**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="9c42b-821">Аспекты представляют собой атрибуты XML, которые предоставляют сведения о том, как значения свойств хранятся в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-822">Аспекты можно применять только к свойствам типа **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-823">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-823">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-824">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="9c42b-825">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-825">Attribute Name</span></span>                                                         | <span data-ttu-id="9c42b-826">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-826">Is Required</span></span> | <span data-ttu-id="9c42b-827">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-828">**Name**</span></span>                                                               | <span data-ttu-id="9c42b-829">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-829">Yes</span></span>         | <span data-ttu-id="9c42b-830">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-831">**Type**</span></span>                                                               | <span data-ttu-id="9c42b-832">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-832">Yes</span></span>         | <span data-ttu-id="9c42b-833">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-833">The type of the property value.</span></span> <span data-ttu-id="9c42b-834">Типом значения свойства должен быть **EDMSimpleType** или сложный тип (обозначается полным именем), который находится в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="9c42b-835">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-835">**Nullable**</span></span>                                                           | <span data-ttu-id="9c42b-836">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-836">No</span></span>          | <span data-ttu-id="9c42b-837">Значение **true** (значение по умолчанию) или <strong>false</strong> в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c42b-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="9c42b-838">> В CSDL v1 свойство сложного типа должно иметь `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-839">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="9c42b-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="9c42b-840">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-840">No</span></span>          | <span data-ttu-id="9c42b-841">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c42b-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="9c42b-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="9c42b-843">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-843">No</span></span>          | <span data-ttu-id="9c42b-844">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="9c42b-846">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-846">No</span></span>          | <span data-ttu-id="9c42b-847">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9c42b-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="9c42b-848">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-848">**Precision**</span></span>                                                          | <span data-ttu-id="9c42b-849">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-849">No</span></span>          | <span data-ttu-id="9c42b-850">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9c42b-851">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-851">**Scale**</span></span>                                                              | <span data-ttu-id="9c42b-852">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-852">No</span></span>          | <span data-ttu-id="9c42b-853">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="9c42b-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-854">**SRID**</span></span>                                                               | <span data-ttu-id="9c42b-855">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-855">No</span></span>          | <span data-ttu-id="9c42b-856">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-857">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9c42b-858">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9c42b-859">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-859">**Unicode**</span></span>                                                            | <span data-ttu-id="9c42b-860">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-860">No</span></span>          | <span data-ttu-id="9c42b-861">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="9c42b-862">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-862">**Collation**</span></span>                                                          | <span data-ttu-id="9c42b-863">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-863">No</span></span>          | <span data-ttu-id="9c42b-864">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="9c42b-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="9c42b-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="9c42b-866">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-866">No</span></span>          | <span data-ttu-id="9c42b-867">**Нет** (значение по умолчанию) или **фиксированное**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="9c42b-868">Если задано значение **fixed**, то значение свойства будет использоваться в проверках оптимистического параллелизма.</span><span class="sxs-lookup"><span data-stu-id="9c42b-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="9c42b-869">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="9c42b-870">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-871">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-872">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-872">Example</span></span>

<span data-ttu-id="9c42b-873">В следующем примере показан элемент **EntityType** с тремя элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="9c42b-874">В следующем примере показан элемент **complexType** с пятью элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="9c42b-875">Применение элемента RowType</span><span class="sxs-lookup"><span data-stu-id="9c42b-875">RowType Element Application</span></span>

<span data-ttu-id="9c42b-876">Элементы **свойств** (как дочерние элементы элемента **RowType** ) определяют форму и характеристики данных, которые могут быть переданы или возвращены из определяемой моделью функции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="9c42b-877">Элемент **Property** может иметь только один из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="9c42b-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="9c42b-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="9c42b-878">CollectionType</span></span>
-   <span data-ttu-id="9c42b-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="9c42b-879">ReferenceType</span></span>
-   <span data-ttu-id="9c42b-880">RowType</span><span class="sxs-lookup"><span data-stu-id="9c42b-880">RowType</span></span>

<span data-ttu-id="9c42b-881">Элемент **Property** может иметь любое число дочерних элементов аннотации.</span><span class="sxs-lookup"><span data-stu-id="9c42b-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-882">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-883">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-883">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-884">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="9c42b-885">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-885">Attribute Name</span></span>                                                     | <span data-ttu-id="9c42b-886">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-886">Is Required</span></span> | <span data-ttu-id="9c42b-887">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-888">**Name**</span></span>                                                           | <span data-ttu-id="9c42b-889">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-889">Yes</span></span>         | <span data-ttu-id="9c42b-890">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-891">**Type**</span></span>                                                           | <span data-ttu-id="9c42b-892">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-892">Yes</span></span>         | <span data-ttu-id="9c42b-893">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-894">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-894">**Nullable**</span></span>                                                       | <span data-ttu-id="9c42b-895">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-895">No</span></span>          | <span data-ttu-id="9c42b-896">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c42b-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="9c42b-897">> В CSDL v1 свойство сложного типа должно иметь `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-898">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="9c42b-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="9c42b-899">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-899">No</span></span>          | <span data-ttu-id="9c42b-900">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c42b-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="9c42b-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="9c42b-902">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-902">No</span></span>          | <span data-ttu-id="9c42b-903">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="9c42b-905">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-905">No</span></span>          | <span data-ttu-id="9c42b-906">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9c42b-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="9c42b-907">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-907">**Precision**</span></span>                                                      | <span data-ttu-id="9c42b-908">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-908">No</span></span>          | <span data-ttu-id="9c42b-909">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9c42b-910">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-910">**Scale**</span></span>                                                          | <span data-ttu-id="9c42b-911">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-911">No</span></span>          | <span data-ttu-id="9c42b-912">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="9c42b-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-913">**SRID**</span></span>                                                           | <span data-ttu-id="9c42b-914">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-914">No</span></span>          | <span data-ttu-id="9c42b-915">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-916">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9c42b-917">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9c42b-918">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-918">**Unicode**</span></span>                                                        | <span data-ttu-id="9c42b-919">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-919">No</span></span>          | <span data-ttu-id="9c42b-920">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="9c42b-921">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-921">**Collation**</span></span>                                                      | <span data-ttu-id="9c42b-922">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-922">No</span></span>          | <span data-ttu-id="9c42b-923">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="9c42b-924">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="9c42b-925">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-926">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="9c42b-927">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-927">Example</span></span>

<span data-ttu-id="9c42b-928">В следующем примере показаны элементы **свойств** , используемые для определения формы возвращаемого типа функции, определяемой моделью.</span><span class="sxs-lookup"><span data-stu-id="9c42b-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="9c42b-929">Элемент PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="9c42b-930">Элемент **PropertyRef** в языке CSDL ссылается на свойство типа сущности, чтобы указать, что свойство будет выполнять одну из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="9c42b-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="9c42b-931">Часть ключа сущности (является свойством или набором свойств типа сущности, которые определяют идентификатор).</span><span class="sxs-lookup"><span data-stu-id="9c42b-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="9c42b-932">Для определения ключа сущности можно использовать один или несколько элементов **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="9c42b-933">Зависимый или основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="9c42b-934">Элемент **PropertyRef** может иметь только элементы аннотации (ноль или более) в качестве дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-935">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-936">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-936">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-937">В следующей таблице описаны атрибуты, которые можно применить к элементу **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="9c42b-938">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-938">Attribute Name</span></span> | <span data-ttu-id="9c42b-939">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-939">Is Required</span></span> | <span data-ttu-id="9c42b-940">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="9c42b-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="9c42b-941">**Name**</span></span>       | <span data-ttu-id="9c42b-942">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-942">Yes</span></span>         | <span data-ttu-id="9c42b-943">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="9c42b-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-944">К элементу **PropertyRef** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="9c42b-945">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-946">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-947">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-947">Example</span></span>

<span data-ttu-id="9c42b-948">В приведенном ниже примере определяется тип сущности (**Книга**).</span><span class="sxs-lookup"><span data-stu-id="9c42b-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="9c42b-949">Ключ сущности определяется с помощью ссылки на свойство **ISBN** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="9c42b-950">В следующем примере используются два элемента **PropertyRef** для указания того, что два свойства (**ID** и **PublisherID**) являются основными и зависимыми элементами ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="9c42b-951">Элемент ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-952">Элемент **ReferenceType** в языке CSDL указывает ссылку на тип сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="9c42b-953">Элемент **ReferenceType** может быть дочерним по отношению к следующим элементам:</span><span class="sxs-lookup"><span data-stu-id="9c42b-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="9c42b-954">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="9c42b-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="9c42b-955">Параметр</span><span class="sxs-lookup"><span data-stu-id="9c42b-955">Parameter</span></span>
-   <span data-ttu-id="9c42b-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="9c42b-956">CollectionType</span></span>

<span data-ttu-id="9c42b-957">Элемент **ReferenceType** используется при определении параметра или возвращаемого типа для функции.</span><span class="sxs-lookup"><span data-stu-id="9c42b-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="9c42b-958">Элемент **ReferenceType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-959">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-960">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-961">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-961">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-962">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="9c42b-963">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-963">Attribute Name</span></span> | <span data-ttu-id="9c42b-964">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-964">Is Required</span></span> | <span data-ttu-id="9c42b-965">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="9c42b-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-966">**Type**</span></span>       | <span data-ttu-id="9c42b-967">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-967">Yes</span></span>         | <span data-ttu-id="9c42b-968">Имя типа сущности, на который делается ссылка.</span><span class="sxs-lookup"><span data-stu-id="9c42b-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-969">К элементу **ReferenceType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="9c42b-970">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-971">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-972">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-972">Example</span></span>

<span data-ttu-id="9c42b-973">В следующем примере показан элемент **ReferenceType** , используемый в качестве дочернего элемента **параметра** в определяемой моделью функции, которая принимает ссылку на тип сущности **Person** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="9c42b-974">В следующем примере показан элемент **ReferenceType** , используемый как дочерний элемент для элемента **ReturnType** (Function) в функции, определяемой моделью, которая возвращает ссылку на тип сущности **Person** :</span><span class="sxs-lookup"><span data-stu-id="9c42b-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="9c42b-975">Элемент ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="9c42b-976">Элемент **ReferentialConstraint** в языке CSDL определяет функциональные возможности, аналогичные ограничению ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="9c42b-977">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="9c42b-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="9c42b-978">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="9c42b-979">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="9c42b-980">Если внешний ключ, представленный в одном типе сущности, ссылается на свойство другого типа сущности, элемент **ReferentialConstraint** определяет ассоциацию между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="9c42b-981">Поскольку элемент **ReferentialConstraint** предоставляет сведения о том, как два типа сущностей связаны друг с друга, соответствующий элемент AssociationSetMapping не требуется в языке спецификации СОПОСТАВЛЕНИЯ (MSL).</span><span class="sxs-lookup"><span data-stu-id="9c42b-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="9c42b-982">Ассоциация между двумя типами сущностей, у которых нет внешних ключей, должна иметь соответствующий элемент **AssociationSetMapping** , чтобы сопоставить сведения о сопоставлении с источником данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="9c42b-983">Если внешний ключ недоступен для типа сущности, элемент **ReferentialConstraint** может определять только ограничение первичного ключа на первичный ключ между типом сущности и другим типом сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="9c42b-984">Элемент **ReferentialConstraint** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-985">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-986">Участник (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="9c42b-987">Зависимый (только один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="9c42b-988">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-989">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-989">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-990">Элемент **ReferentialConstraint** может иметь любое количество атрибутов аннотации (НАСТРАИВАЕМЫЕ атрибуты XML).</span><span class="sxs-lookup"><span data-stu-id="9c42b-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="9c42b-991">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-992">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-993">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-993">Example</span></span>

<span data-ttu-id="9c42b-994">В следующем примере показан элемент **ReferentialConstraint** , используемый как часть определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="9c42b-995">Элемент ReturnType (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="9c42b-996">Элемент **ReturnType** (Function) в языке CSDL указывает тип возвращаемого значения для функции, которая определена в элементе Function.</span><span class="sxs-lookup"><span data-stu-id="9c42b-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="9c42b-997">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="9c42b-998">Возвращаемыми типами могут быть любой **EDMSimpleType**, тип сущности, сложный тип, тип строки, ссылочный тип или коллекция одного из этих типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="9c42b-999">Тип возвращаемого значения функции может быть указан либо с помощью атрибута **Type** элемента **ReturnType** (Function), либо с помощью одного из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="9c42b-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="9c42b-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1000">CollectionType</span></span>
-   <span data-ttu-id="9c42b-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1001">ReferenceType</span></span>
-   <span data-ttu-id="9c42b-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-1003">Модель не будет проверяться, если указать возвращаемый тип функции с атрибутом **Type** элемента **ReturnType** (Function) и одного из дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1004">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1004">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1005">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="9c42b-1006">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-1006">Attribute Name</span></span> | <span data-ttu-id="9c42b-1007">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-1007">Is Required</span></span> | <span data-ttu-id="9c42b-1008">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="9c42b-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1009">**ReturnType**</span></span> | <span data-ttu-id="9c42b-1010">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1010">No</span></span>          | <span data-ttu-id="9c42b-1011">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-1012">К элементу **ReturnType** (Function) можно применить любое количество атрибутов аннотации (пользовательские XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="9c42b-1013">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1014">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-1015">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1015">Example</span></span>

<span data-ttu-id="9c42b-1016">В следующем примере используется элемент **Function** для определения функции, возвращающей число лет, в течение которых книга была напечатана.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="9c42b-1017">Обратите внимание, что тип возвращаемого значения задается атрибутом **Type** элемента **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="9c42b-1018">Элемент ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="9c42b-1019">Элемент **ReturnType** (FunctionImport) в языке CSDL указывает тип возвращаемого значения для функции, которая определена в элементе FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="9c42b-1020">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="9c42b-1021">Возвращаемые типы могут быть любой коллекцией типа сущности, сложного типа или **EDMSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="9c42b-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="9c42b-1022">Тип возвращаемого значения функции указывается с помощью атрибута **Type** элемента **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1023">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1023">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1024">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="9c42b-1025">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-1025">Attribute Name</span></span> | <span data-ttu-id="9c42b-1026">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-1026">Is Required</span></span> | <span data-ttu-id="9c42b-1027">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1028">**Type**</span></span>       | <span data-ttu-id="9c42b-1029">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1029">No</span></span>          | <span data-ttu-id="9c42b-1030">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1030">The type that the function returns.</span></span> <span data-ttu-id="9c42b-1031">Значение должно быть коллекцией типа ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="9c42b-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1032">**EntitySet**</span></span>  | <span data-ttu-id="9c42b-1033">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1033">No</span></span>          | <span data-ttu-id="9c42b-1034">Если функция возвращает коллекцию типов сущностей, то значение **EntitySet** должно быть набором сущностей, к которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="9c42b-1035">В противном случае не следует использовать атрибут **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-1036">К элементу **ReturnType** (FunctionImport) можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="9c42b-1037">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1038">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-1039">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1039">Example</span></span>

<span data-ttu-id="9c42b-1040">В следующем примере используется функция **FunctionImport** , возвращающая книги и издатели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="9c42b-1041">Обратите внимание, что функция возвращает два результирующих набора и, следовательно, указаны два элемента **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="9c42b-1042">Элемент RowType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="9c42b-1043">Элемент **RowType** в языке CSDL определяет неименованную структуру как параметр или возвращаемый тип для функции, определенной в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="9c42b-1044">Элемент **RowType** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="9c42b-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1045">CollectionType</span></span>
-   <span data-ttu-id="9c42b-1046">Параметр</span><span class="sxs-lookup"><span data-stu-id="9c42b-1046">Parameter</span></span>
-   <span data-ttu-id="9c42b-1047">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1047">ReturnType (Function)</span></span>

<span data-ttu-id="9c42b-1048">Элемент **RowType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-1049">Свойство (одно или несколько)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1049">Property (one or more)</span></span>
-   <span data-ttu-id="9c42b-1050">Элементы аннотации (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1051">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1051">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1052">К элементу **RowType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="9c42b-1053">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1054">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-1055">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1055">Example</span></span>

<span data-ttu-id="9c42b-1056">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="9c42b-1057">Элемент Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="9c42b-1058">Элемент **Schema** является корневым элементом определения концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="9c42b-1059">Он содержит определения объектов, функций и контейнеров, из которых состоит концептуальная модель.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="9c42b-1060">Элемент **Schema** может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="9c42b-1061">Using</span><span class="sxs-lookup"><span data-stu-id="9c42b-1061">Using</span></span>
-   <span data-ttu-id="9c42b-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="9c42b-1062">EntityContainer</span></span>
-   <span data-ttu-id="9c42b-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1063">EntityType</span></span>
-   <span data-ttu-id="9c42b-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1064">EnumType</span></span>
-   <span data-ttu-id="9c42b-1065">Ассоциация</span><span class="sxs-lookup"><span data-stu-id="9c42b-1065">Association</span></span>
-   <span data-ttu-id="9c42b-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1066">ComplexType</span></span>
-   <span data-ttu-id="9c42b-1067">Функция</span><span class="sxs-lookup"><span data-stu-id="9c42b-1067">Function</span></span>

<span data-ttu-id="9c42b-1068">Элемент **схемы** может содержать не более одного элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-1069">Элементы **Function** и Annotation разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="9c42b-1070">Элемент **Schema** использует атрибут **Namespace** для определения пространства имен для объектов типа сущности, сложного типа и ассоциаций в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="9c42b-1071">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="9c42b-1072">Пространства имен могут охватывать несколько элементов **схемы** и несколько CSDL-файлов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="9c42b-1073">Пространство имен концептуальной модели отличается от пространства имен XML элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="9c42b-1074">Пространство имен концептуальной модели (как определено атрибутом **пространства имен** ) является логическим контейнером для типов сущностей, сложных типов и типов взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="9c42b-1075">Пространство имен XML (обозначенное атрибутом **xmlns** ) элемента **Schema** является пространством имен по умолчанию для дочерних элементов и атрибутов элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="9c42b-1076">Пространства имен XML формы https://schemas.microsoft.com/ado/YYYY/MM/edm (где гггг и MM представляют год и месяц соответственно) зарезервированы для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1077">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1078">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1078">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1079">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **схемы** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="9c42b-1080">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-1080">Attribute Name</span></span> | <span data-ttu-id="9c42b-1081">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-1081">Is Required</span></span> | <span data-ttu-id="9c42b-1082">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-1083">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1083">**Namespace**</span></span>  | <span data-ttu-id="9c42b-1084">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1084">Yes</span></span>         | <span data-ttu-id="9c42b-1085">Пространство имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="9c42b-1086">Значение атрибута **Namespace** используется для формирования полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="9c42b-1087">Например, если **EntityType** с именем *Customer* находится в пространстве имен Simple. example. Model, то полное имя **EntityType** — симпликсамплемодел. Customer.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="9c42b-1088">Следующие строки не могут использоваться в качестве значения для атрибута **Namespace** : **Системная**, **временная**или **EDM**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="9c42b-1089">Значение атрибута **пространства имен** не может совпадать со значением атрибута **Namespace** в элементе схемы SSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="9c42b-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1090">**Alias**</span></span>      | <span data-ttu-id="9c42b-1091">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1091">No</span></span>          | <span data-ttu-id="9c42b-1092">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="9c42b-1093">Например, если **EntityType** с именем *Customer* находится в пространстве имен Simple. пример. Model, а значение атрибута **Alias** — *model*, то можно использовать модель. Customer как полное имя **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="9c42b-1094">К элементу **схемы** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="9c42b-1095">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1096">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-1097">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1097">Example</span></span>

<span data-ttu-id="9c42b-1098">В следующем примере показан элемент **схемы** , содержащий элемент **EntityContainer** , два элемента **EntityType** и один элемент **Association** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="9c42b-1099">Элемент TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="9c42b-1100">Элемент **TypeRef** в языке CSDL предоставляет ссылку на существующий именованный тип.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="9c42b-1101">Элемент **TypeRef** может быть дочерним по отношению к элементу CollectionType, который используется для указания того, что функция имеет коллекцию в качестве параметра или возвращаемого типа.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="9c42b-1102">Элемент **TypeRef** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9c42b-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9c42b-1103">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9c42b-1104">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1105">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1105">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1106">В следующей таблице описаны атрибуты, которые можно применить к элементу **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="9c42b-1107">Обратите внимание, что атрибуты **DefaultValue**, **MaxLength**, **FixedLength**, **точность**, **масштаб**, **Юникод**и **Параметры сортировки** применимы только к **едмсимплетипес**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="9c42b-1108">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="9c42b-1109">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-1109">Is Required</span></span> | <span data-ttu-id="9c42b-1110">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1111">**Type**</span></span>                                                           | <span data-ttu-id="9c42b-1112">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1112">No</span></span>          | <span data-ttu-id="9c42b-1113">Имя типа, на который дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="9c42b-1114">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="9c42b-1115">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1115">No</span></span>          | <span data-ttu-id="9c42b-1116">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="9c42b-1117">> В CSDL v1 свойство сложного типа должно иметь `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="9c42b-1118">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="9c42b-1119">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1119">No</span></span>          | <span data-ttu-id="9c42b-1120">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="9c42b-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="9c42b-1122">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1122">No</span></span>          | <span data-ttu-id="9c42b-1123">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="9c42b-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="9c42b-1125">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1125">No</span></span>          | <span data-ttu-id="9c42b-1126">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="9c42b-1127">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1127">**Precision**</span></span>                                                      | <span data-ttu-id="9c42b-1128">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1128">No</span></span>          | <span data-ttu-id="9c42b-1129">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9c42b-1130">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1130">**Scale**</span></span>                                                          | <span data-ttu-id="9c42b-1131">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1131">No</span></span>          | <span data-ttu-id="9c42b-1132">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="9c42b-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1133">**SRID**</span></span>                                                           | <span data-ttu-id="9c42b-1134">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1134">No</span></span>          | <span data-ttu-id="9c42b-1135">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9c42b-1136">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9c42b-1137">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9c42b-1138">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="9c42b-1139">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1139">No</span></span>          | <span data-ttu-id="9c42b-1140">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="9c42b-1141">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1141">**Collation**</span></span>                                                      | <span data-ttu-id="9c42b-1142">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1142">No</span></span>          | <span data-ttu-id="9c42b-1143">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="9c42b-1144">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="9c42b-1145">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1146">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-1147">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1147">Example</span></span>

<span data-ttu-id="9c42b-1148">В следующем примере показана определяемая моделью функция, использующая элемент **TypeRef** (как дочерний элемент элемента **CollectionType** ), чтобы указать, что функция принимает коллекцию типов сущностей **отдела** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="9c42b-1149">Элемент Using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="9c42b-1150">Элемент **using** в языке CSDL импортирует содержимое концептуальной модели, которая существует в другом пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="9c42b-1151">Устанавливая значение атрибута **Namespace** , можно ссылаться на типы сущностей, сложные типы и типы взаимосвязей, определенные в другой концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="9c42b-1152">Более одного элемента **using** может быть потомком элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-1153">Элемент **using** в языке CSDL не работает точно так же, как оператор **using** в языке программирования.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="9c42b-1154">Импорт пространства имен с помощью оператора **using** в языке программирования не влияет на объекты в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="9c42b-1155">В языке CSDL импортированное пространство имен может содержать тип сущности, производный от типа сущности в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="9c42b-1156">Это может влиять на наборы сущностей, объявленные в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="9c42b-1157">Элемент **using** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="9c42b-1158">Документация (разрешены ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="9c42b-1159">Элементы аннотации (разрешены ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9c42b-1160">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1160">Applicable Attributes</span></span>

<span data-ttu-id="9c42b-1161">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **using** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="9c42b-1162">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9c42b-1162">Attribute Name</span></span> | <span data-ttu-id="9c42b-1163">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9c42b-1163">Is Required</span></span> | <span data-ttu-id="9c42b-1164">Значение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-1165">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1165">**Namespace**</span></span>  | <span data-ttu-id="9c42b-1166">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1166">Yes</span></span>         | <span data-ttu-id="9c42b-1167">Имя импортируемого пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="9c42b-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1168">**Alias**</span></span>      | <span data-ttu-id="9c42b-1169">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1169">Yes</span></span>         | <span data-ttu-id="9c42b-1170">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="9c42b-1171">Хотя этот атрибут является обязательным, необязательно, чтобы он использовался вместо имени пространства имен при указании имен объектов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="9c42b-1172">К элементу **using** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="9c42b-1173">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9c42b-1174">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="9c42b-1175">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1175">Example</span></span>

<span data-ttu-id="9c42b-1176">В следующем примере показан элемент **using** , используемый для импорта пространства имен, определенного в других местах.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="9c42b-1177">Обратите внимание, что пространство имен для элемента **схемы** показано `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="9c42b-1178">Свойство `Address` типа**EntityType** `Publisher` является сложным типом, который определен в пространстве имен `ExtendedBooksModel` (импортированном с **помощью** элемента).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="9c42b-1179">Атрибуты примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="9c42b-1180">Атрибуты annotation в языке CSDL представляют собой настраиваемые атрибуты XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="9c42b-1181">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="9c42b-1182">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="9c42b-1183">К данному конкретному элементу языка CSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="9c42b-1184">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="9c42b-1185">Атрибуты annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="9c42b-1186">Метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-1187">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1187">Example</span></span>

<span data-ttu-id="9c42b-1188">В следующем примере показан элемент **EntityType** с атрибутом заметки (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="9c42b-1189">В следующем примере показано применение элемента annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="9c42b-1190">Следующий код извлекает метаданные из атрибута annotation и выводит их на консоль:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="9c42b-1191">Вышеуказанный код предполагает, что файл `School.csdl` находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="9c42b-1192">Элементы примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="9c42b-1193">Элементы annotation в языке CSDL представляют собой настраиваемые элементы XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="9c42b-1194">Элементы annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="9c42b-1195">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="9c42b-1196">У данного элемента языка CSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="9c42b-1197">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="9c42b-1198">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="9c42b-1199">Элементы annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="9c42b-1200">Начиная с .NET Framework версии 4 метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-1201">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1201">Example</span></span>

<span data-ttu-id="9c42b-1202">В следующем примере показан элемент **EntityType** с элементом annotation (**кустомелемент**).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="9c42b-1203">В следующем примере показано применение атрибута annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="9c42b-1204">Следующий код извлекает метаданные из элемента annotation и выводит их на консоль.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="9c42b-1205">Вышеприведенный код предполагает, что файл School.csdl находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="9c42b-1206">Типы концептуальной модели (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="9c42b-1207">Язык CSDL поддерживает набор абстрактных примитивных типов данных, именуемых **едмсимплетипес**, которые определяют свойства в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="9c42b-1208">**Едмсимплетипес** — это прокси-серверы для примитивных типов данных, которые поддерживаются в среде хранения или размещения.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="9c42b-1209">В приведенной ниже таблице представлены примитивные типы данных, поддерживаемые CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="9c42b-1210">В таблице также перечислены аспекты, которые можно применять к каждому **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="9c42b-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="9c42b-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="9c42b-1212">Описание</span><span class="sxs-lookup"><span data-stu-id="9c42b-1212">Description</span></span>                                                | <span data-ttu-id="9c42b-1213">Применимые аспекты</span><span class="sxs-lookup"><span data-stu-id="9c42b-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="9c42b-1214">**EDM. binary**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="9c42b-1215">Содержит двоичные данные.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="9c42b-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="9c42b-1217">**EDM. Boolean**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="9c42b-1218">Содержит значение **true** или **false**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="9c42b-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="9c42b-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="9c42b-1221">Содержит 8-битное целое значение без знака.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="9c42b-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="9c42b-1224">Представляет дату и время.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="9c42b-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1226">**EDM. DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="9c42b-1227">Возвращает дату и время в виде смещения в минутах от времени GMT.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="9c42b-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="9c42b-1230">Содержит точное числовое значение с заданной точностью и масштабом.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="9c42b-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="9c42b-1233">Содержит число с плавающей запятой с точностью до 15 цифр</span><span class="sxs-lookup"><span data-stu-id="9c42b-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="9c42b-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="9c42b-1236">Содержит число с плавающей запятой с точностью до 7 знаков.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="9c42b-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="9c42b-1239">Содержит уникальный 16-битный идентификатор.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="9c42b-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="9c42b-1242">Содержит 16-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="9c42b-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1244">**EDM. Int32**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="9c42b-1245">Содержит 32-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="9c42b-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1247">**EDM. Int64**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="9c42b-1248">Содержит 64-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="9c42b-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="9c42b-1251">Содержит 8-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="9c42b-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1253">**Edm.String**</span></span>                   | <span data-ttu-id="9c42b-1254">Содержит символьные данные.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1254">Contains character data.</span></span>                                   | <span data-ttu-id="9c42b-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="9c42b-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="9c42b-1257">Содержит время дня.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="9c42b-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="9c42b-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="9c42b-1259">**Модель EDM. geography**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="9c42b-1260">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1261">**EDM. GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="9c42b-1262">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="9c42b-1264">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="9c42b-1266">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="9c42b-1268">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="9c42b-1270">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="9c42b-1272">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1273">**EDM. Жеографиколлектион**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="9c42b-1274">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="9c42b-1276">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="9c42b-1278">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="9c42b-1280">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="9c42b-1282">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="9c42b-1284">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="9c42b-1286">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="9c42b-1288">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="9c42b-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="9c42b-1290">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="9c42b-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="9c42b-1291">Области (CSDL)</span><span class="sxs-lookup"><span data-stu-id="9c42b-1291">Facets (CSDL)</span></span>

<span data-ttu-id="9c42b-1292">Аспекты в языке CSDL представляют ограничения для свойств типов сущности и сложных типов.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="9c42b-1293">Аспекты выглядят как XML-атрибуты в следующих элементах CSDL:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="9c42b-1294">Свойство</span><span class="sxs-lookup"><span data-stu-id="9c42b-1294">Property</span></span>
-   <span data-ttu-id="9c42b-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="9c42b-1295">TypeRef</span></span>
-   <span data-ttu-id="9c42b-1296">Параметр</span><span class="sxs-lookup"><span data-stu-id="9c42b-1296">Parameter</span></span>

<span data-ttu-id="9c42b-1297">В следующей таблице описываются аспекты, поддерживаемые в CSDL.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="9c42b-1298">Все аспекты являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1298">All facets are optional.</span></span> <span data-ttu-id="9c42b-1299">Некоторые аспекты, перечисленные ниже, используются Entity Framework при создании базы данных из концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="9c42b-1300">Сведения о типах данных в концептуальной модели см. в разделе Типы концептуальной модели (CSDL).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="9c42b-1301">Аспект</span><span class="sxs-lookup"><span data-stu-id="9c42b-1301">Facet</span></span>               | <span data-ttu-id="9c42b-1302">Описание</span><span class="sxs-lookup"><span data-stu-id="9c42b-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="9c42b-1303">Применение</span><span class="sxs-lookup"><span data-stu-id="9c42b-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="9c42b-1304">Используется для создания базы данных</span><span class="sxs-lookup"><span data-stu-id="9c42b-1304">Used for the database generation</span></span> | <span data-ttu-id="9c42b-1305">Используется средой выполнения</span><span class="sxs-lookup"><span data-stu-id="9c42b-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="9c42b-1306">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1306">**Collation**</span></span>       | <span data-ttu-id="9c42b-1307">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="9c42b-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="9c42b-1309">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1309">Yes</span></span>                              | <span data-ttu-id="9c42b-1310">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1310">No</span></span>                  |
| <span data-ttu-id="9c42b-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="9c42b-1312">Указывает, что значение свойства должно использоваться в проверках оптимистического управления параллелизмом.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="9c42b-1313">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="9c42b-1314">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1314">No</span></span>                               | <span data-ttu-id="9c42b-1315">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1315">Yes</span></span>                 |
| <span data-ttu-id="9c42b-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1316">**Default**</span></span>         | <span data-ttu-id="9c42b-1317">Задает значение по умолчанию для свойства в случае, если при создании экземпляра не было задано значение.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="9c42b-1318">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="9c42b-1319">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1319">Yes</span></span>                              | <span data-ttu-id="9c42b-1320">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1320">Yes</span></span>                 |
| <span data-ttu-id="9c42b-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1321">**FixedLength**</span></span>     | <span data-ttu-id="9c42b-1322">Указывает, может ли изменяться длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="9c42b-1323">**EDM. binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="9c42b-1324">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1324">Yes</span></span>                              | <span data-ttu-id="9c42b-1325">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1325">No</span></span>                  |
| <span data-ttu-id="9c42b-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1326">**MaxLength**</span></span>       | <span data-ttu-id="9c42b-1327">Указывает максимальную длину значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="9c42b-1328">**EDM. binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="9c42b-1329">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1329">Yes</span></span>                              | <span data-ttu-id="9c42b-1330">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1330">No</span></span>                  |
| <span data-ttu-id="9c42b-1331">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1331">**Nullable**</span></span>        | <span data-ttu-id="9c42b-1332">Указывает, может ли свойство иметь значение **null** .</span><span class="sxs-lookup"><span data-stu-id="9c42b-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="9c42b-1333">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="9c42b-1334">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1334">Yes</span></span>                              | <span data-ttu-id="9c42b-1335">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1335">Yes</span></span>                 |
| <span data-ttu-id="9c42b-1336">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1336">**Precision**</span></span>       | <span data-ttu-id="9c42b-1337">Для свойств типа **Decimal**указывает количество цифр, которое может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="9c42b-1338">Для свойств типа **time**, **DateTime**и **DateTimeOffset**указывает количество цифр для дробной части секунды значения свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="9c42b-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **модель EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="9c42b-1340">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1340">Yes</span></span>                              | <span data-ttu-id="9c42b-1341">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1341">No</span></span>                  |
| <span data-ttu-id="9c42b-1342">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1342">**Scale**</span></span>           | <span data-ttu-id="9c42b-1343">Задает число цифр справа от десятичной запятой в значении свойства.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="9c42b-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="9c42b-1345">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1345">Yes</span></span>                              | <span data-ttu-id="9c42b-1346">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1346">No</span></span>                  |
| <span data-ttu-id="9c42b-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1347">**SRID**</span></span>            | <span data-ttu-id="9c42b-1348">Указывает идентификатор системы пространственных ссылок на систему.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="9c42b-1349">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c42b-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="9c42b-1350">**EDM. geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. Жеографиколлектион, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="9c42b-1351">Нет</span><span class="sxs-lookup"><span data-stu-id="9c42b-1351">No</span></span>                               | <span data-ttu-id="9c42b-1352">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1352">Yes</span></span>                 |
| <span data-ttu-id="9c42b-1353">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1353">**Unicode**</span></span>         | <span data-ttu-id="9c42b-1354">Указывает, будет ли значение свойства храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="9c42b-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="9c42b-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="9c42b-1356">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1356">Yes</span></span>                              | <span data-ttu-id="9c42b-1357">Да</span><span class="sxs-lookup"><span data-stu-id="9c42b-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="9c42b-1358">При создании базы данных из концептуальной модели мастер создания базы данных распознает значение атрибута **StoreGeneratedPattern** в элементе **Property** , если оно находится в следующем пространстве имен: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="9c42b-1359">Для атрибута поддерживаются значения **Identity** и **COMPUTE**.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="9c42b-1360">Значение **Identity** приведет к созданию столбца базы данных со значением идентификатора, созданным в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="9c42b-1361">Значение **вычисляемое** приведет к созданию столбца со значением, вычисляемым в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c42b-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="9c42b-1362">Пример</span><span class="sxs-lookup"><span data-stu-id="9c42b-1362">Example</span></span>

<span data-ttu-id="9c42b-1363">В следующем примере показывается применение аспектов к свойствам типа сущности:</span><span class="sxs-lookup"><span data-stu-id="9c42b-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
