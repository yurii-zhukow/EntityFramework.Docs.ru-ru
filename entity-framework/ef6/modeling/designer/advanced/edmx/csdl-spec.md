---
title: Спецификация языка CSDL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
caps.latest.revision: 3
ms.openlocfilehash: 0ece73a19fe7ea244905bccb728ab2a104c5179f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122398"
---
# <a name="csdl-specification"></a><span data-ttu-id="d25c4-102">Спецификация CSDL</span><span class="sxs-lookup"><span data-stu-id="d25c4-102">CSDL Specification</span></span>
<span data-ttu-id="d25c4-103">Язык CSDL — это язык на основе XML, описывающий сущности, связи и функции, составляющие концептуальную модель управляемого данными приложения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="d25c4-104">Эту концептуальную модель можно использовать с Entity Framework или службы WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="d25c4-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="d25c4-105">Метаданные, описываемые на языке CSDL используется платформой Entity Framework для сопоставления сущностей и связей, которые определены в концептуальной модели к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="d25c4-106">Дополнительные сведения см. в разделе [спецификация SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) и [спецификация MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="d25c4-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="d25c4-107">CSDL — это реализация модели EDM Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d25c4-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="d25c4-108">В приложении Entity Framework метаданные концептуальной модели, загружается из CSDL-файл (написанный на языке CSDL) в экземпляр System.Data.Metadata.Edm.EdmItemCollection и доступен с помощью методов в Класс System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="d25c4-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="d25c4-109">Метаданные концептуальной модели Entity Framework используются для преобразования запросов к концептуальной модели в команды, зависящие от источника данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="d25c4-110">Конструктор EF хранит сведения о концептуальной модели в EDMX-файла во время разработки.</span><span class="sxs-lookup"><span data-stu-id="d25c4-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="d25c4-111">Во время сборки, EF конструктор на основе сведений в EDMX-файла, создаваемого CSDL-файл, необходимый Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="d25c4-112">Версии языка CSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="d25c4-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="d25c4-113">Версия языка CSDL</span><span class="sxs-lookup"><span data-stu-id="d25c4-113">CSDL Version</span></span> | <span data-ttu-id="d25c4-114">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="d25c4-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="d25c4-115">CSDL версии 1</span><span class="sxs-lookup"><span data-stu-id="d25c4-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="d25c4-116">CSDL версии 2</span><span class="sxs-lookup"><span data-stu-id="d25c4-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="d25c4-117">CSDL версии 3</span><span class="sxs-lookup"><span data-stu-id="d25c4-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="d25c4-118">Элемент Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-118">Association Element (CSDL)</span></span>

<span data-ttu-id="d25c4-119">**Ассоциации** элемент определяет связь между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="d25c4-120">Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью.</span><span class="sxs-lookup"><span data-stu-id="d25c4-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="d25c4-121">Кратность элемента ассоциации может иметь значение (1), ноль или один (0.. 1) или многие (\*).</span><span class="sxs-lookup"><span data-stu-id="d25c4-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="d25c4-122">Эта информация указывается в два дочерних элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="d25c4-123">К экземплярам типов сущностей в одном элементе ассоциации можно обращаться посредством свойств навигации или внешних ключей, если они предоставлены в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="d25c4-124">В приложении экземпляр ассоциации представляет конкретную ассоциацию между экземплярами типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="d25c4-125">Экземпляры ассоциации логически сгруппированы в набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="d25c4-126">**Ассоциации** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-127">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-128">End (ровно 2 элемента)</span><span class="sxs-lookup"><span data-stu-id="d25c4-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="d25c4-129">ReferentialConstraint (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-130">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-131">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-131">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-132">В следующей таблице описаны атрибуты, которые могут применяться к **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="d25c4-133">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-133">Attribute Name</span></span> | <span data-ttu-id="d25c4-134">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-134">Is Required</span></span> | <span data-ttu-id="d25c4-135">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="d25c4-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-136">**Name**</span></span>       | <span data-ttu-id="d25c4-137">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-137">Yes</span></span>         | <span data-ttu-id="d25c4-138">Имя ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-139">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="d25c4-140">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-141">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-142">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-142">Example</span></span>

<span data-ttu-id="d25c4-143">В следующем примере показан **ассоциации** элемент, определяющий **CustomerOrders** ассоциации, если внешние ключи не представлены в **клиента** и  **Порядок** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="d25c4-144">**Кратность** значения для каждого **конечных** ассоциации указывают что многие **заказы** может быть связано с **клиента**, но только один **клиента** может быть связано с **порядок**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="d25c4-145">Кроме того **OnDelete** элемент указывает, что все **заказы** , относящихся к конкретному **клиента** и были загружены в контекст ObjectContext будут удалены Если **клиента** удаляется.</span><span class="sxs-lookup"><span data-stu-id="d25c4-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="d25c4-146">В следующем примере показан **ассоциации** элемент, определяющий **CustomerOrders** ассоциации, если внешние ключи представлены в **клиента** и  **Порядок** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="d25c4-147">Внешние ключи представлены, связи между сущностями, управляются с помощью **ReferentialConstraint** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="d25c4-148">Соответствующий элемент AssociationSetMapping необязателен для сопоставления этой ассоциации с источником данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="d25c4-149">Элемент AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="d25c4-150">**AssociationSet** элемента на языке определения концептуальной схемы (CSDL) — это логический контейнер для экземпляров ассоциаций одного типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="d25c4-151">Набор ассоциаций предоставляет определение группы экземпляров ассоциаций, чтобы их можно было сопоставить с источником данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="d25c4-152">**AssociationSet** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-153">Документация (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-154">End (требуется ровно 2 элемента)</span><span class="sxs-lookup"><span data-stu-id="d25c4-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="d25c4-155">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="d25c4-156">**Ассоциации** атрибут задает тип ассоциации, содержащейся в наборе ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="d25c4-157">Наборы сущностей, которые представляют конечные набора ассоциаций, указываются ровно двумя дочерними **окончания** элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-158">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-158">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-159">В следующей таблице описаны атрибуты, которые могут применяться к **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="d25c4-160">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-160">Attribute Name</span></span>  | <span data-ttu-id="d25c4-161">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-161">Is Required</span></span> | <span data-ttu-id="d25c4-162">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-163">**Name**</span></span>        | <span data-ttu-id="d25c4-164">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-164">Yes</span></span>         | <span data-ttu-id="d25c4-165">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-165">The name of the entity set.</span></span> <span data-ttu-id="d25c4-166">Значение **имя** атрибут не может быть таким же, как значение **ассоциации** атрибута.</span><span class="sxs-lookup"><span data-stu-id="d25c4-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="d25c4-167">**Ассоциации**</span><span class="sxs-lookup"><span data-stu-id="d25c4-167">**Association**</span></span> | <span data-ttu-id="d25c4-168">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-168">Yes</span></span>         | <span data-ttu-id="d25c4-169">Полное имя ассоциации, экземпляры которой содержатся в наборе ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="d25c4-170">Ассоциация должна находиться в том же пространстве имен, что и набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-171">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="d25c4-172">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-173">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-174">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-174">Example</span></span>

<span data-ttu-id="d25c4-175">В следующем примере показан **EntityContainer** элемент с двумя **AssociationSet** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="d25c4-176">Элемент CollectionType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-177">**CollectionType** элемента на языке определения концептуальной схемы (CSDL) указывает, что параметр функции или функции возвращает тип является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="d25c4-178">**CollectionType** элемент может быть дочерним элемент параметра или элемент ReturnType (функция).</span><span class="sxs-lookup"><span data-stu-id="d25c4-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="d25c4-179">Тип коллекции можно указать с помощью **тип** атрибут или одно из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="d25c4-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-180">**CollectionType**</span></span>
-   <span data-ttu-id="d25c4-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d25c4-181">ReferenceType</span></span>
-   <span data-ttu-id="d25c4-182">RowType</span><span class="sxs-lookup"><span data-stu-id="d25c4-182">RowType</span></span>
-   <span data-ttu-id="d25c4-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="d25c4-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-184">Модель не пройдет проверку, если тип коллекции будет указан и с **тип** атрибут и дочерний элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-185">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-185">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-186">В следующей таблице описаны атрибуты, которые могут применяться к **CollectionType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d25c4-187">Обратите внимание, что **DefaultValue**, **MaxLength**, **FixedLength**, **точности**, **масштабирования**,  **Юникод**, и **параметры сортировки** атрибуты применимы только к коллекциям **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="d25c4-188">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-188">Attribute Name</span></span>                                                          | <span data-ttu-id="d25c4-189">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-189">Is Required</span></span> | <span data-ttu-id="d25c4-190">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-191">**Type**</span></span>                                                                | <span data-ttu-id="d25c4-192">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-192">No</span></span>          | <span data-ttu-id="d25c4-193">Тип коллекции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d25c4-194">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-194">**Nullable**</span></span>                                                            | <span data-ttu-id="d25c4-195">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-195">No</span></span>          | <span data-ttu-id="d25c4-196">**Значение true,** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="d25c4-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="d25c4-197">> В языке CSDL v1, должен иметь свойство сложного типа `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="d25c4-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="d25c4-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="d25c4-199">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-199">No</span></span>          | <span data-ttu-id="d25c4-200">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d25c4-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="d25c4-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="d25c4-202">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-202">No</span></span>          | <span data-ttu-id="d25c4-203">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="d25c4-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="d25c4-205">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-205">No</span></span>          | <span data-ttu-id="d25c4-206">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="d25c4-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="d25c4-207">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-207">**Precision**</span></span>                                                           | <span data-ttu-id="d25c4-208">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-208">No</span></span>          | <span data-ttu-id="d25c4-209">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="d25c4-210">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-210">**Scale**</span></span>                                                               | <span data-ttu-id="d25c4-211">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-211">No</span></span>          | <span data-ttu-id="d25c4-212">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-213">**SRID**</span></span>                                                                | <span data-ttu-id="d25c4-214">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-214">No</span></span>          | <span data-ttu-id="d25c4-215">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-216">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="d25c4-217">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="d25c4-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="d25c4-218">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-218">**Unicode**</span></span>                                                             | <span data-ttu-id="d25c4-219">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-219">No</span></span>          | <span data-ttu-id="d25c4-220">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="d25c4-221">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-221">**Collation**</span></span>                                                           | <span data-ttu-id="d25c4-222">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-222">No</span></span>          | <span data-ttu-id="d25c4-223">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="d25c4-224">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **CollectionType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d25c4-225">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-226">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-227">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-227">Example</span></span>

<span data-ttu-id="d25c4-228">В следующем примере показано определяемой моделью функции, в которой используется **CollectionType** элемента, чтобы указать, что функция возвращает коллекцию **Person** типы сущностей (как указано с **ElementType** атрибут).</span><span class="sxs-lookup"><span data-stu-id="d25c4-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="d25c4-229">В следующем примере показано определяемой моделью функции, который использует **CollectionType** элемента, чтобы указать, что функция возвращает коллекцию строк (как указано в **RowType** элемент).</span><span class="sxs-lookup"><span data-stu-id="d25c4-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="d25c4-230">В следующем примере показано определяемой моделью функции, который использует **CollectionType** элемента, чтобы указать, что функция принимает в качестве параметра коллекцию **отдел** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="d25c4-231">Элемент ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-232">Объект **ComplexType** элемент определяет структуру данных, состоящих из **EdmSimpleType** свойств или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="d25c4-233">Сложный тип может быть свойством типа сущности или другого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="d25c4-234">Сложный тип аналогичен типу сущности, поскольку также определяет данные.</span><span class="sxs-lookup"><span data-stu-id="d25c4-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="d25c4-235">Однако между сложными типами и типами сущности существуют некоторые ключевые различия.</span><span class="sxs-lookup"><span data-stu-id="d25c4-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="d25c4-236">Сложные типы не имеют идентификаторов (или ключей) и поэтому не могут существовать независимо.</span><span class="sxs-lookup"><span data-stu-id="d25c4-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="d25c4-237">Сложные типы могут существовать только как свойства типов сущностей или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="d25c4-238">Сложные типы не могут участвовать в связях.</span><span class="sxs-lookup"><span data-stu-id="d25c4-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="d25c4-239">Ни конечной точки ассоциации может быть сложным типом, и поэтому свойства навигации нельзя определить для сложных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="d25c4-240">Свойство сложного типа не может иметь значение null, хотя каждое скалярное свойство сложного типа может быть установлено в это значение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="d25c4-241">Объект **ComplexType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-242">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-243">Свойство (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="d25c4-244">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="d25c4-245">В следующей таблице описаны атрибуты, которые могут применяться к **ComplexType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="d25c4-246">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="d25c4-247">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-247">Is Required</span></span> | <span data-ttu-id="d25c4-248">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-249">name</span><span class="sxs-lookup"><span data-stu-id="d25c4-249">Name</span></span>                                                                                                           | <span data-ttu-id="d25c4-250">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-250">Yes</span></span>         | <span data-ttu-id="d25c4-251">Имя сложного типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-251">The name of the complex type.</span></span> <span data-ttu-id="d25c4-252">Имя сложного типа не может совпадать с именем другого сложного типа, типа сущности или сопоставления, которые находятся в области модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="d25c4-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="d25c4-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="d25c4-254">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-254">No</span></span>          | <span data-ttu-id="d25c4-255">Имя другого сложного типа, который является базовым типом определяемого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="d25c4-256">> Этот атрибут неприменим в версии 1 для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="d25c4-257">В этой версии не поддерживается наследование для сложных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="d25c4-258">Абстрактный</span><span class="sxs-lookup"><span data-stu-id="d25c4-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="d25c4-259">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-259">No</span></span>          | <span data-ttu-id="d25c4-260">**Значение true,** или **False** (значение по умолчанию) в зависимости от того, является ли сложный тип абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="d25c4-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="d25c4-261">> Этот атрибут неприменим в версии 1 для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="d25c4-262">Сложные типы в этой версии не могут быть абстрактными типами.</span><span class="sxs-lookup"><span data-stu-id="d25c4-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="d25c4-263">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ComplexType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="d25c4-264">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-265">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-266">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-266">Example</span></span>

<span data-ttu-id="d25c4-267">В следующем примере показано сложный тип, **адрес**, с помощью **EdmSimpleType** свойства **StreetAddress**, **Город**,  **StateOrProvince**, **страны**, и **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="d25c4-268">Чтобы определить сложный тип **адрес** (выше) как свойство типа сущности, необходимо объявить тип свойства в определении типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="d25c4-269">В следующем примере показан **адрес** свойство как сложный тип для типа сущности (**издателя**):</span><span class="sxs-lookup"><span data-stu-id="d25c4-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="d25c4-270">Элемент DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="d25c4-271">**DefiningExpression** элемент на языке определения концептуальной схемы (CSDL) содержит выражение Entity SQL, которая определяет функцию в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="d25c4-272">В целях проверки **DefiningExpression** элемент может содержать произвольное содержимое.</span><span class="sxs-lookup"><span data-stu-id="d25c4-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="d25c4-273">Тем не менее, платформа Entity Framework приведет к возникновению исключения во время выполнения при **DefiningExpression** элемент не содержит допустимый язык Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-274">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-274">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-275">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **DefiningExpression** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="d25c4-276">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-277">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-278">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-278">Example</span></span>

<span data-ttu-id="d25c4-279">В следующем примере используется **DefiningExpression** для определения функции, которая возвращает количество лет с момента публикации книги.</span><span class="sxs-lookup"><span data-stu-id="d25c4-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="d25c4-280">Содержание **DefiningExpression** элемент создается на языке Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="d25c4-281">Элемент Dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="d25c4-282">**Зависимых** элемента на языке определения концептуальной схемы (CSDL) является дочерним элементом для элемента ReferentialConstraint и определяет зависимый конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="d25c4-283">Объект **ReferentialConstraint** элемент определяет функциональные возможности, схожие с возможностями ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d25c4-284">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d25c4-285">Тип сущности, на который приведена ссылка называется *основного конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d25c4-286">Тип сущности, который ссылается на основном конечном элементе называется *зависимого конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="d25c4-287">**PropertyRef** элементы, используемые для указания, какие разделы ссылаются на основной элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="d25c4-288">**Зависимых** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-289">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d25c4-290">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-291">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-291">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-292">В следующей таблице описаны атрибуты, которые могут применяться к **зависимых** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="d25c4-293">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-293">Attribute Name</span></span> | <span data-ttu-id="d25c4-294">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-294">Is Required</span></span> | <span data-ttu-id="d25c4-295">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="d25c4-296">**Роль**</span><span class="sxs-lookup"><span data-stu-id="d25c4-296">**Role**</span></span>       | <span data-ttu-id="d25c4-297">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-297">Yes</span></span>         | <span data-ttu-id="d25c4-298">Имя типа сущности в зависимом элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-299">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **зависимых** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="d25c4-300">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-301">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-302">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-302">Example</span></span>

<span data-ttu-id="d25c4-303">В следующем примере показан **ReferentialConstraint** элемента, используемого в качестве части определения **PublishedBy** ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="d25c4-304">**PublisherId** свойство **книги** делает тип сущности представляет зависимый элемент ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="d25c4-305">Элемент Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="d25c4-306">**Документации** элемента на языке определения концептуальной схемы (CSDL) можно использовать для предоставления сведений об объекте, который определен в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="d25c4-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="d25c4-307">В файле EDMX когда **документации** элемент является дочерним для элемента, который отображается в виде объекта в области конструктора в конструкторе EF (например, сущность, ассоциация или свойство), содержимое **документации**  элемент будет отображаться в Visual Studio **свойства** для объекта.</span><span class="sxs-lookup"><span data-stu-id="d25c4-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="d25c4-308">**Документации** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-309">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="d25c4-310">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-310">(zero or one element)</span></span>
-   <span data-ttu-id="d25c4-311">**LongDescription**: подробное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="d25c4-312">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-312">(zero or one element)</span></span>
-   <span data-ttu-id="d25c4-313">Элементы annotation.</span><span class="sxs-lookup"><span data-stu-id="d25c4-313">Annotation elements.</span></span> <span data-ttu-id="d25c4-314">(ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-315">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-315">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-316">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **документации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="d25c4-317">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-318">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-319">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-319">Example</span></span>

<span data-ttu-id="d25c4-320">В следующем примере показан **документации** элемент как дочерний элемент элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="d25c4-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="d25c4-321">Если приведенный ниже фрагмент кода на языке CSDL содержимое EDMX, содержимое файла из **Сводка** и **LongDescription** появилось бы в Visual Studio **свойства** окно, при щелчке `Customer` типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="d25c4-322">Элемент End (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-322">End Element (CSDL)</span></span>

<span data-ttu-id="d25c4-323">**Окончания** элемент на языке определения концептуальной схемы (CSDL) может быть потомком элемента Association или элемент AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="d25c4-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="d25c4-324">В каждом случае роль **окончания** отличается элемент и применимые атрибуты отличаются.</span><span class="sxs-lookup"><span data-stu-id="d25c4-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="d25c4-325">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="d25c4-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="d25c4-326">**Окончания** элемент (как дочерний **ассоциации** элемент) определяет тип сущности на одном конце ассоциации и число экземпляров типа сущности, которые могут существовать на одной конечной точке ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="d25c4-327">Элементы ассоциации определяются при определении ассоциации; ассоциация должна иметь два элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="d25c4-328">К экземплярам типов сущности на одном элементе ассоциации можно осуществлять доступ с помощью свойств навигации или внешних ключей при условии, что они были предоставлены в типах сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="d25c4-329">**Окончания** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-330">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-331">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-332">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-333">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-333">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-334">В следующей таблице описаны атрибуты, которые могут применяться к **окончания** элементу, если он является дочерним элементом **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="d25c4-335">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-335">Attribute Name</span></span>   | <span data-ttu-id="d25c4-336">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-336">Is Required</span></span> | <span data-ttu-id="d25c4-337">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-338">**Type**</span></span>         | <span data-ttu-id="d25c4-339">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-339">Yes</span></span>         | <span data-ttu-id="d25c4-340">Имя типа сущности на одном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="d25c4-341">**Роль**</span><span class="sxs-lookup"><span data-stu-id="d25c4-341">**Role**</span></span>         | <span data-ttu-id="d25c4-342">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-342">No</span></span>          | <span data-ttu-id="d25c4-343">Имя для элемента ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-343">A name for the association end.</span></span> <span data-ttu-id="d25c4-344">Если имя не было предоставлено, будет использовано имя типа сущности на элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="d25c4-345">**Кратность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-345">**Multiplicity**</span></span> | <span data-ttu-id="d25c4-346">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-346">Yes</span></span>         | <span data-ttu-id="d25c4-347">**1**, **от 0 до 1**, или **\*** в зависимости от количества экземпляров типа сущности, которые могут быть в конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="d25c4-348">**1** означает, что только один экземпляр типа сущности существует на элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="d25c4-349">**от 0 до 1** указывает, что на конечной точке ассоциации существует ноль или один экземпляров типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="d25c4-350">**\*** Указывает, что на конечной точке ассоциации существует ноль, один или несколько экземпляров типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-351">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **окончания** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="d25c4-352">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-353">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-354">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-354">Example</span></span>

<span data-ttu-id="d25c4-355">В следующем примере показан **ассоциации** элемент, определяющий **CustomerOrders** ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="d25c4-356">**Кратность** значения для каждого **конечных** ассоциации указывают что многие **заказы** может быть связано с **клиента**, но только один **клиента** может быть связано с **порядок**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="d25c4-357">Кроме того **OnDelete** элемент указывает, что все **заказы** , относящихся к конкретному **клиента** и которые были загружены в контекст ObjectContext будет Если удаленные **клиента** удаляется.</span><span class="sxs-lookup"><span data-stu-id="d25c4-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="d25c4-358">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="d25c4-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="d25c4-359">**Окончания** элемент указывает один конец набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="d25c4-360">**AssociationSet** элемент должен содержать два **окончания** элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="d25c4-361">Сведения, содержащиеся в **окончания** элемент используется в сопоставлении с источником данных набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="d25c4-362">**Окончания** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-363">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-364">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-365">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="d25c4-366">Элементы annotation допускаются только в том случае, в языке CSDL версии 2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d25c4-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-367">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-367">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-368">В следующей таблице описаны атрибуты, которые могут применяться к **окончания** элементу, если он является дочерним элементом **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="d25c4-369">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-369">Attribute Name</span></span> | <span data-ttu-id="d25c4-370">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-370">Is Required</span></span> | <span data-ttu-id="d25c4-371">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-372">**Набор EntitySet**</span><span class="sxs-lookup"><span data-stu-id="d25c4-372">**EntitySet**</span></span>  | <span data-ttu-id="d25c4-373">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-373">Yes</span></span>         | <span data-ttu-id="d25c4-374">Имя **EntitySet** , определяющего один элемент родительского элемента **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="d25c4-375">**EntitySet** должен быть определен в том же контейнере сущностей, что и родительский элемент **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="d25c4-376">**Роль**</span><span class="sxs-lookup"><span data-stu-id="d25c4-376">**Role**</span></span>       | <span data-ttu-id="d25c4-377">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-377">No</span></span>          | <span data-ttu-id="d25c4-378">Имя элемента набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-378">The name of the association set end.</span></span> <span data-ttu-id="d25c4-379">Если **роли** атрибут не используется, имя набора ассоциаций будет использоваться имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d25c4-380">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **окончания** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="d25c4-381">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-382">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-383">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-383">Example</span></span>

<span data-ttu-id="d25c4-384">В следующем примере показан **EntityContainer** элемент с двумя **AssociationSet** элементов, каждый с двумя **окончания** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="d25c4-385">Элемент EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="d25c4-386">**EntityContainer** элемента на языке определения концептуальной схемы (CSDL) — это логический контейнер для наборов сущностей, наборов ассоциаций и функций импорта.</span><span class="sxs-lookup"><span data-stu-id="d25c4-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="d25c4-387">Контейнер сущностей модели хранения посредством элемента EntityContainerMapping сопоставляет контейнер сущностей концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="d25c4-388">Контейнер сущностей режима хранения описывает структуру базы данных: наборы сущностей описывают таблицы, наборы ассоциаций описывают ограничения внешних ключей, функции импорта описывают хранимые процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="d25c4-389">**EntityContainer** элемент может иметь ноль или один элемент документации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="d25c4-390">Если **документации** элемент присутствует, он должен предшествовать **EntitySet**, **AssociationSet**, и **FunctionImport** элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="d25c4-391">**EntityContainer** элемент может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="d25c4-392">EntitySet</span></span>
-   <span data-ttu-id="d25c4-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="d25c4-393">AssociationSet</span></span>
-   <span data-ttu-id="d25c4-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="d25c4-394">FunctionImport</span></span>
-   <span data-ttu-id="d25c4-395">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="d25c4-395">Annotation elements</span></span>

<span data-ttu-id="d25c4-396">Вы можете расширить **EntityContainer** элемент для включения содержимого другого **EntityContainer** , находящегося в одном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="d25c4-397">Чтобы включить содержимое другого **EntityContainer**, в ссылающейся **EntityContainer** элемент, установите для параметра **расширения** атрибут имени  **EntityContainer** элемент, который требуется включить.</span><span class="sxs-lookup"><span data-stu-id="d25c4-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="d25c4-398">Все дочерние элементы включенного **EntityContainer** элемент будет рассматриваться как дочерние элементы ссылающегося **EntityContainer** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-399">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-399">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-400">В следующей таблице описаны атрибуты, которые могут применяться к **Using** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="d25c4-401">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-401">Attribute Name</span></span> | <span data-ttu-id="d25c4-402">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-402">Is Required</span></span> | <span data-ttu-id="d25c4-403">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="d25c4-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-404">**Name**</span></span>       | <span data-ttu-id="d25c4-405">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-405">Yes</span></span>         | <span data-ttu-id="d25c4-406">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="d25c4-407">**Расширяет**</span><span class="sxs-lookup"><span data-stu-id="d25c4-407">**Extends**</span></span>    | <span data-ttu-id="d25c4-408">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-408">No</span></span>          | <span data-ttu-id="d25c4-409">Имя другого контейнера сущностей в том же пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-410">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntityContainer** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="d25c4-411">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-412">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-413">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-413">Example</span></span>

<span data-ttu-id="d25c4-414">В следующем примере показан **EntityContainer** элемент, определяющий три набора сущностей и два набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="d25c4-415">Элемент EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="d25c4-416">**EntitySet** элемент в язык определения концептуальной схемы — это логический контейнер для экземпляров типа сущности и экземпляров любого типа, который является производным от этого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="d25c4-417">Связь между типом сущности и набором сущностей аналогична связи между строкой и таблицей в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="d25c4-418">Тип сущности, как и строка, определяет ряд взаимосвязанных данных, а набор сущностей, как и таблица, содержит экземпляры этого определения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="d25c4-419">Набор сущностей предоставляет конструкцию для группирования экземпляров типа сущности, чтобы их можно было сопоставлять со связанными структурами данных в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="d25c4-420">Можно определить больше одного набора сущностей для конкретного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-421">Конструктор EF не поддерживает концептуальные модели, которые содержат несколько наборов сущностей на тип.</span><span class="sxs-lookup"><span data-stu-id="d25c4-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="d25c4-422">**EntitySet** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-423">Элемент Documentation (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-424">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-425">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-425">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-426">В следующей таблице описаны атрибуты, которые могут применяться к **EntitySet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="d25c4-427">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-427">Attribute Name</span></span> | <span data-ttu-id="d25c4-428">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-428">Is Required</span></span> | <span data-ttu-id="d25c4-429">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-430">**Name**</span></span>       | <span data-ttu-id="d25c4-431">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-431">Yes</span></span>         | <span data-ttu-id="d25c4-432">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="d25c4-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-433">**EntityType**</span></span> | <span data-ttu-id="d25c4-434">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-434">Yes</span></span>         | <span data-ttu-id="d25c4-435">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="d25c4-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-436">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntitySet** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="d25c4-437">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-438">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-439">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-439">Example</span></span>

<span data-ttu-id="d25c4-440">В следующем примере показан **EntityContainer** элемент с тремя **EntitySet** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="d25c4-441">Предусмотрена возможность определять несколько наборов сущностей на тип (модель MEST).</span><span class="sxs-lookup"><span data-stu-id="d25c4-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="d25c4-442">В следующем примере определяется контейнер сущностей с двумя наборами сущностей для **книги** типа сущности:</span><span class="sxs-lookup"><span data-stu-id="d25c4-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="d25c4-443">Элемент EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-444">**EntityType** элемент представляет структуру концепции верхнего уровня, например, клиента или заказа, в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="d25c4-445">Тип сущности — это шаблон для экземпляров типов сущностей в приложении.</span><span class="sxs-lookup"><span data-stu-id="d25c4-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="d25c4-446">Каждый шаблон содержит следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="d25c4-447">Уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="d25c4-447">A unique name.</span></span> <span data-ttu-id="d25c4-448">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="d25c4-448">(Required.)</span></span>
-   <span data-ttu-id="d25c4-449">Ключ сущности, определяемый одним или несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="d25c4-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="d25c4-450">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="d25c4-450">(Required.)</span></span>
-   <span data-ttu-id="d25c4-451">Свойства содержащихся данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-451">Properties for containing data.</span></span> <span data-ttu-id="d25c4-452">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="d25c4-452">(Optional.)</span></span>
-   <span data-ttu-id="d25c4-453">Свойства навигации, позволяющие осуществлять переход от одного элемента ассоциации к другому.</span><span class="sxs-lookup"><span data-stu-id="d25c4-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="d25c4-454">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="d25c4-454">(Optional.)</span></span>

<span data-ttu-id="d25c4-455">В приложении экземпляр типа сущности представляет определенный объект (например, определенного клиента или заказ).</span><span class="sxs-lookup"><span data-stu-id="d25c4-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="d25c4-456">Каждый экземпляр типа сущности в наборе сущностей должен иметь уникальный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="d25c4-457">Два экземпляра типа сущности считаются равными, только если они являются экземплярами одного типа и значения их ключей сущности равны.</span><span class="sxs-lookup"><span data-stu-id="d25c4-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="d25c4-458">**EntityType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-459">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-460">Ключ (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-461">Свойство (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="d25c4-462">NavigationProperty (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="d25c4-463">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-464">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-464">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-465">В следующей таблице описаны атрибуты, которые могут применяться к **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="d25c4-466">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="d25c4-467">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-467">Is Required</span></span> | <span data-ttu-id="d25c4-468">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="d25c4-470">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-470">Yes</span></span>         | <span data-ttu-id="d25c4-471">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="d25c4-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="d25c4-473">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-473">No</span></span>          | <span data-ttu-id="d25c4-474">Имя другого типа сущности, являющегося базовым типом для определяемого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="d25c4-475">**Абстрактный**</span><span class="sxs-lookup"><span data-stu-id="d25c4-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="d25c4-476">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-476">No</span></span>          | <span data-ttu-id="d25c4-477">**Значение true,** или **False**в зависимости от того, является ли тип сущности абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="d25c4-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="d25c4-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="d25c4-479">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-479">No</span></span>          | <span data-ttu-id="d25c4-480">**Значение true,** или **False** в зависимости от того, является ли тип сущности является тип сущности открытым.</span><span class="sxs-lookup"><span data-stu-id="d25c4-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="d25c4-481">> **OpenType** атрибут применяется только к типам сущностей, которые определены в концептуальной модели, которые используются со службами данных ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="d25c4-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="d25c4-482">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="d25c4-483">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-484">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-485">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-485">Example</span></span>

<span data-ttu-id="d25c4-486">В следующем примере показан **EntityType** элемент с тремя **свойство** элементов и два **NavigationProperty** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="d25c4-487">Элемент EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-488">**EnumType** элемент представляет собой перечислимый тип.</span><span class="sxs-lookup"><span data-stu-id="d25c4-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="d25c4-489">**EnumType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-490">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-491">Член (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="d25c4-492">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-493">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-493">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-494">В следующей таблице описаны атрибуты, которые могут применяться к **EnumType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="d25c4-495">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-495">Attribute Name</span></span>     | <span data-ttu-id="d25c4-496">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-496">Is Required</span></span> | <span data-ttu-id="d25c4-497">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-498">**Name**</span></span>           | <span data-ttu-id="d25c4-499">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-499">Yes</span></span>         | <span data-ttu-id="d25c4-500">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="d25c4-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="d25c4-501">**IsFlags**</span></span>        | <span data-ttu-id="d25c4-502">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-502">No</span></span>          | <span data-ttu-id="d25c4-503">**Значение true,** или **False**, в зависимости от типа перечисления может использоваться как набор флагов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="d25c4-504">Значение по умолчанию — **False.**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="d25c4-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-505">**UnderlyingType**</span></span> | <span data-ttu-id="d25c4-506">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-506">No</span></span>          | <span data-ttu-id="d25c4-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** или **Edm.SByte** Определение диапазона значений типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="d25c4-508">Является базовым типом перечисления элементов по умолчанию **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-509">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EnumType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="d25c4-510">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-511">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-512">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-512">Example</span></span>

<span data-ttu-id="d25c4-513">В следующем примере показан **EnumType** элемент с тремя **член** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="d25c4-514">Элемент Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-514">Function Element (CSDL)</span></span>

<span data-ttu-id="d25c4-515">**Функция** элемент на языке определения концептуальной схемы (CSDL) используется для определения или объявления функций в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="d25c4-516">Функция определяется с помощью элемент DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="d25c4-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="d25c4-517">Объект **функция** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-518">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-519">Параметр (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="d25c4-520">DefiningExpression (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-521">ReturnType (Function) (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-522">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="d25c4-523">Возвращает объект типа для функции должен быть указан с помощью **ReturnType** элемент (Function) или **ReturnType** атрибутов (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="d25c4-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="d25c4-524">Возвращаемым типом может быть EdmSimpleType, тип сущности, сложный тип, строковый тип, ссылочный тип или коллекция, которая включает один из этих типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-525">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-525">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-526">В следующей таблице описаны атрибуты, которые могут применяться к **функция** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="d25c4-527">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-527">Attribute Name</span></span> | <span data-ttu-id="d25c4-528">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-528">Is Required</span></span> | <span data-ttu-id="d25c4-529">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="d25c4-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-530">**Name**</span></span>       | <span data-ttu-id="d25c4-531">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-531">Yes</span></span>         | <span data-ttu-id="d25c4-532">Имя функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-532">The name of the function.</span></span>          |
| <span data-ttu-id="d25c4-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-533">**ReturnType**</span></span> | <span data-ttu-id="d25c4-534">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-534">No</span></span>          | <span data-ttu-id="d25c4-535">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-536">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **функция** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="d25c4-537">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-538">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-539">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-539">Example</span></span>

<span data-ttu-id="d25c4-540">В следующем примере используется **функция** элемент, чтобы определить функцию, которая возвращает количество лет, так как инструктор был принят на работу.</span><span class="sxs-lookup"><span data-stu-id="d25c4-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="d25c4-541">Элемент FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="d25c4-542">**FunctionImport** элемент на языке определения концептуальной схемы (CSDL) представляет функции, определенные в источнике данных, но доступна для объектов через концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="d25c4-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="d25c4-543">Например элемента Function в модели хранения можно использовать для представления хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="d25c4-544">Объект **FunctionImport** элемента в концептуальной модели представляет соответствующую функцию в приложении Entity Framework и сопоставляется с функцией модели хранения с помощью элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="d25c4-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="d25c4-545">При вызове функции в приложении соответствующая хранимая процедура выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="d25c4-546">**FunctionImport** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-547">Документация (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-548">Параметр (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="d25c4-549">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="d25c4-550">ReturnType (FunctionImport) (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="d25c4-551">Один **параметр** элемент должен быть определен для каждого параметра, принимаемого функцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="d25c4-552">Возвращает объект типа для функции должен быть указан с помощью **ReturnType** элемент (FunctionImport) или **ReturnType** атрибутов (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="d25c4-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="d25c4-553">Типа возвращаемого значения должен быть коллекцией EdmSimpleType, тип EntityType или ComplexType.</span><span class="sxs-lookup"><span data-stu-id="d25c4-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-554">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-554">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-555">В следующей таблице описаны атрибуты, которые могут применяться к **FunctionImport** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="d25c4-556">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-556">Attribute Name</span></span>   | <span data-ttu-id="d25c4-557">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-557">Is Required</span></span> | <span data-ttu-id="d25c4-558">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-559">**Name**</span></span>         | <span data-ttu-id="d25c4-560">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-560">Yes</span></span>         | <span data-ttu-id="d25c4-561">Имя импортируемой функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="d25c4-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-562">**ReturnType**</span></span>   | <span data-ttu-id="d25c4-563">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-563">No</span></span>          | <span data-ttu-id="d25c4-564">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-564">The type that the function returns.</span></span> <span data-ttu-id="d25c4-565">Не используйте этот атрибут, если функция не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="d25c4-566">В противном случае значение должно быть коллекция ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="d25c4-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="d25c4-567">**Набор EntitySet**</span><span class="sxs-lookup"><span data-stu-id="d25c4-567">**EntitySet**</span></span>    | <span data-ttu-id="d25c4-568">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-568">No</span></span>          | <span data-ttu-id="d25c4-569">Если функция возвращает коллекцию сущностей типов, значение **EntitySet** должен представлять собой набор сущностей, которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="d25c4-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="d25c4-570">В противном случае **EntitySet** атрибут не должен использоваться.</span><span class="sxs-lookup"><span data-stu-id="d25c4-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="d25c4-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="d25c4-571">**IsComposable**</span></span> | <span data-ttu-id="d25c4-572">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-572">No</span></span>          | <span data-ttu-id="d25c4-573">Если присвоено значение true, функция — композицию (Table-valued Function) и может использоваться в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="d25c4-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="d25c4-574">Значение по умолчанию — **false**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="d25c4-575">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **FunctionImport** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="d25c4-576">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-577">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-578">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-578">Example</span></span>

<span data-ttu-id="d25c4-579">В следующем примере показан **FunctionImport** элемент, который принимает один параметр и возвращает коллекцию типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="d25c4-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="d25c4-580">Элемент Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-580">Key Element (CSDL)</span></span>

<span data-ttu-id="d25c4-581">**Ключ** элемент является дочерним элементом элемента EntityType и определяет *ключ сущности* (свойство или набор свойств типа сущности, которые определяют идентификатор).</span><span class="sxs-lookup"><span data-stu-id="d25c4-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="d25c4-582">Свойства, составляющие ключ сущности, выбираются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="d25c4-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="d25c4-583">Значения свойств ключа сущности должны уникально определять экземпляр типа сущности внутри набора сущностей во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="d25c4-584">Свойства, составляющие ключ сущности, должны гарантировать уникальность экземпляра набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="d25c4-585">**Ключ** элемент определяет ключ сущности, ссылаясь на одно или несколько свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="d25c4-586">**Ключ** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="d25c4-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="d25c4-587">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d25c4-588">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-589">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-589">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-590">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ключ** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="d25c4-591">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-592">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-593">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-593">Example</span></span>

<span data-ttu-id="d25c4-594">В приведенном ниже примере определяется тип сущности с именем **книги**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="d25c4-595">Ключ сущности определяется ссылкой на **ISBN** свойство типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="d25c4-596">**ISBN** свойство является хорошим выбором для ключа сущности, поскольку международный стандартный номер книги (ISBN) уникально определяет книгу.</span><span class="sxs-lookup"><span data-stu-id="d25c4-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="d25c4-597">В следующем примере показано типа сущности (**автор**), имеет ключ сущности, состоящий из двух свойств **имя** и **адрес**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="d25c4-598">С помощью **имя** и **адрес** для сущности, ключ является разумным выбором, поскольку двух авторов с тем же именем, скорее всего, live по тому же адресу.</span><span class="sxs-lookup"><span data-stu-id="d25c4-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="d25c4-599">Однако такой выбор ключа сущности не гарантирует уникальность ключей сущности в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="d25c4-600">Добавление свойства, такие как **AuthorId**, который может использоваться для уникальной идентификации автора в этом случае рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="d25c4-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="d25c4-601">Элемент Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-601">Member Element (CSDL)</span></span>

<span data-ttu-id="d25c4-602">**Член** элемент является дочерним элементом элемента EnumType и определяет член перечисляемого типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-603">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-603">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-604">В следующей таблице описаны атрибуты, которые могут применяться к **FunctionImport** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="d25c4-605">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-605">Attribute Name</span></span> | <span data-ttu-id="d25c4-606">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-606">Is Required</span></span> | <span data-ttu-id="d25c4-607">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-608">**Name**</span></span>       | <span data-ttu-id="d25c4-609">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-609">Yes</span></span>         | <span data-ttu-id="d25c4-610">Имя элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="d25c4-611">**Значение**</span><span class="sxs-lookup"><span data-stu-id="d25c4-611">**Value**</span></span>      | <span data-ttu-id="d25c4-612">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-612">No</span></span>          | <span data-ttu-id="d25c4-613">Значение элемента.</span><span class="sxs-lookup"><span data-stu-id="d25c4-613">The value of the member.</span></span> <span data-ttu-id="d25c4-614">По умолчанию первый элемент имеет значение 0, и значение каждого последующего перечислителя увеличивается на 1.</span><span class="sxs-lookup"><span data-stu-id="d25c4-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="d25c4-615">Может существовать несколько членов с одинаковыми значениями.</span><span class="sxs-lookup"><span data-stu-id="d25c4-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-616">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **FunctionImport** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="d25c4-617">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-618">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-619">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-619">Example</span></span>

<span data-ttu-id="d25c4-620">В следующем примере показан **EnumType** элемент с тремя **член** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="d25c4-621">Элемент NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="d25c4-622">Объект **NavigationProperty** элемент определяет свойство навигации, который предоставляет ссылку на другом конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="d25c4-623">В отличие от свойства, определенные в элементе свойства свойства навигации не определяют форму и характеристики данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="d25c4-624">Они предоставляют возможность навигации по ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="d25c4-625">Обратите внимание, что свойства навигации являются необязательными для обоих типов сущностей, расположенных в конечных элементах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="d25c4-626">Если свойство навигации было определено для типа сущности на одном конечном элементе ассоциации, то определять его для типа сущности на другом конечном элементе необязательно.</span><span class="sxs-lookup"><span data-stu-id="d25c4-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="d25c4-627">Тип данных, возвращаемый свойством навигации, определяется кратностью в удаленном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="d25c4-628">Например, предположим, что свойство навигации, **OrdersNavProp**, существует на **клиента** типа сущности и устанавливает связь "один ко многим" между **клиента** и  **Порядок**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="d25c4-629">Так как конечная точка удаленной ассоциации для свойства навигации имеет кратность многие (\*), он имеет тип данных коллекции (из **порядок**).</span><span class="sxs-lookup"><span data-stu-id="d25c4-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="d25c4-630">Аналогичным образом, если свойство навигации, **CustomerNavProp**, существует на **порядок** будет иметь тип сущности, тип данных **клиента** поскольку кратность удаленного конечного один (1).</span><span class="sxs-lookup"><span data-stu-id="d25c4-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="d25c4-631">Объект **NavigationProperty** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-632">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-633">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-634">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-634">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-635">В следующей таблице описаны атрибуты, которые могут применяться к **NavigationProperty** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="d25c4-636">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-636">Attribute Name</span></span>   | <span data-ttu-id="d25c4-637">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-637">Is Required</span></span> | <span data-ttu-id="d25c4-638">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-639">**Name**</span></span>         | <span data-ttu-id="d25c4-640">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-640">Yes</span></span>         | <span data-ttu-id="d25c4-641">Имя свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="d25c4-642">**Связь**</span><span class="sxs-lookup"><span data-stu-id="d25c4-642">**Relationship**</span></span> | <span data-ttu-id="d25c4-643">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-643">Yes</span></span>         | <span data-ttu-id="d25c4-644">Имя ассоциации, расположенной в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="d25c4-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="d25c4-645">**ToRole**</span></span>       | <span data-ttu-id="d25c4-646">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-646">Yes</span></span>         | <span data-ttu-id="d25c4-647">Конечная точка ассоциации на которой заканчивается навигация.</span><span class="sxs-lookup"><span data-stu-id="d25c4-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="d25c4-648">Значение **ToRole** атрибут должен быть таким же, как значение одного из **роли** атрибутами, определенными на одном из окончаний ассоциации (определенных в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="d25c4-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="d25c4-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="d25c4-649">**FromRole**</span></span>     | <span data-ttu-id="d25c4-650">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-650">Yes</span></span>         | <span data-ttu-id="d25c4-651">Конечная точка ассоциации с которой начинается навигация.</span><span class="sxs-lookup"><span data-stu-id="d25c4-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="d25c4-652">Значение **FromRole** атрибут должен быть таким же, как значение одного из **роли** атрибутами, определенными на одном из окончаний ассоциации (определенных в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="d25c4-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-653">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **NavigationProperty** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="d25c4-654">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-655">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-656">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-656">Example</span></span>

<span data-ttu-id="d25c4-657">В следующем примере определяется тип сущности (**книги**) с двумя свойствами навигации (**PublishedBy** и **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="d25c4-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="d25c4-658">Элемент OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="d25c4-659">**OnDelete** элемент на языке определения концептуальной схемы (CSDL) определяет поведение, связанное с ассоциацией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="d25c4-660">Если **действие** атрибуту присваивается **Cascade** на одном конце ассоциации, связанные с типами сущностей на другой стороне ассоциации удаляются при удалении типа сущности в первом элементе.</span><span class="sxs-lookup"><span data-stu-id="d25c4-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="d25c4-661">Если связь между двумя типами сущностей связь «первичный ключ к первичному ключа», то загруженный зависимый объект удаляется при удалении главного объекта, на другом конце ассоциации вне зависимости от **OnDelete** Спецификация.</span><span class="sxs-lookup"><span data-stu-id="d25c4-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="d25c4-662">**OnDelete** элемент влияет только на поведение во время выполнения приложения; он не влияет на поведение в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="d25c4-663">Поведение, определенное в источнике данных, должно совпадать с поведением, определенным для приложения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="d25c4-664">**OnDelete** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-665">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-666">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-667">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-667">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-668">В следующей таблице описаны атрибуты, которые могут применяться к **OnDelete** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="d25c4-669">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-669">Attribute Name</span></span> | <span data-ttu-id="d25c4-670">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-670">Is Required</span></span> | <span data-ttu-id="d25c4-671">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-672">**Действие**</span><span class="sxs-lookup"><span data-stu-id="d25c4-672">**Action**</span></span>     | <span data-ttu-id="d25c4-673">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-673">Yes</span></span>         | <span data-ttu-id="d25c4-674">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-674">**Cascade** or **None**.</span></span> <span data-ttu-id="d25c4-675">Если **Cascade**, зависимые типы сущностей будут удалены при удалении основного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="d25c4-676">Если **None**, зависимые типы сущностей не будут удалены при удалении основного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-677">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="d25c4-678">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-679">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-680">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-680">Example</span></span>

<span data-ttu-id="d25c4-681">В следующем примере показан **ассоциации** элемент, определяющий **CustomerOrders** ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="d25c4-682">**OnDelete** элемент указывает, что все **заказы** , относящихся к конкретному **клиента** и были загружены в контекст ObjectContext будут удалены при  **Клиент** удаляется.</span><span class="sxs-lookup"><span data-stu-id="d25c4-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="d25c4-683">Элемент Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="d25c4-684">**Параметр** элемент на языке определения концептуальной схемы (CSDL) может быть дочерний элемент FunctionImport, либо функция.</span><span class="sxs-lookup"><span data-stu-id="d25c4-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="d25c4-685">Приложение, использующее элемент FunctionImport</span><span class="sxs-lookup"><span data-stu-id="d25c4-685">FunctionImport Element Application</span></span>

<span data-ttu-id="d25c4-686">Объект **параметр** элемент (как дочерний **FunctionImport** элемент) используется для определения входных и выходных параметров функций импорта, объявленных на языке CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="d25c4-687">**Параметр** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-688">Документация (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-689">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-690">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-690">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-691">В следующей таблице описаны атрибуты, которые могут применяться к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="d25c4-692">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-692">Attribute Name</span></span> | <span data-ttu-id="d25c4-693">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-693">Is Required</span></span> | <span data-ttu-id="d25c4-694">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-695">**Name**</span></span>       | <span data-ttu-id="d25c4-696">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-696">Yes</span></span>         | <span data-ttu-id="d25c4-697">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d25c4-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-698">**Type**</span></span>       | <span data-ttu-id="d25c4-699">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-699">Yes</span></span>         | <span data-ttu-id="d25c4-700">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-700">The parameter type.</span></span> <span data-ttu-id="d25c4-701">Значение должно быть **EDMSimpleType** или сложный тип, который находится в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="d25c4-702">**Режим**</span><span class="sxs-lookup"><span data-stu-id="d25c4-702">**Mode**</span></span>       | <span data-ttu-id="d25c4-703">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-703">No</span></span>          | <span data-ttu-id="d25c4-704">**В**, **Out**, или **InOut** в зависимости от того, является ли параметр входной, выходной или параметр ввода вывода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="d25c4-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-705">**MaxLength**</span></span>  | <span data-ttu-id="d25c4-706">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-706">No</span></span>          | <span data-ttu-id="d25c4-707">Максимально допустимая длина параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="d25c4-708">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-708">**Precision**</span></span>  | <span data-ttu-id="d25c4-709">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-709">No</span></span>          | <span data-ttu-id="d25c4-710">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-711">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-711">**Scale**</span></span>      | <span data-ttu-id="d25c4-712">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-712">No</span></span>          | <span data-ttu-id="d25c4-713">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="d25c4-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-714">**SRID**</span></span>       | <span data-ttu-id="d25c4-715">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-715">No</span></span>          | <span data-ttu-id="d25c4-716">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-717">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="d25c4-718">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-719">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="d25c4-720">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-721">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-722">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-722">Example</span></span>

<span data-ttu-id="d25c4-723">В следующем примере показан **FunctionImport** элемент с одним **параметр** дочерний элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="d25c4-724">Функция принимает один входной параметр и возвращает коллекцию типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="d25c4-725">Приложение, использующее элемент Function</span><span class="sxs-lookup"><span data-stu-id="d25c4-725">Function Element Application</span></span>

<span data-ttu-id="d25c4-726">Объект **параметр** элемент (как дочерний **функция** элемент) определяет параметры для функций, определяемых или объявляемых в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="d25c4-727">**Параметр** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-728">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="d25c4-729">CollectionType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="d25c4-730">ReferenceType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="d25c4-731">RowType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-732">Только один из **CollectionType**, **ReferenceType**, или **RowType** элементов может быть дочерним элементом элемента **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="d25c4-733">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-734">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="d25c4-735">Элементы annotation допускаются только в том случае, в языке CSDL версии 2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d25c4-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-736">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-736">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-737">В следующей таблице описаны атрибуты, которые могут применяться к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="d25c4-738">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-738">Attribute Name</span></span>   | <span data-ttu-id="d25c4-739">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-739">Is Required</span></span> | <span data-ttu-id="d25c4-740">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-741">**Name**</span></span>         | <span data-ttu-id="d25c4-742">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-742">Yes</span></span>         | <span data-ttu-id="d25c4-743">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d25c4-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-744">**Type**</span></span>         | <span data-ttu-id="d25c4-745">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-745">No</span></span>          | <span data-ttu-id="d25c4-746">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="d25c4-746">The parameter type.</span></span> <span data-ttu-id="d25c4-747">Параметр может иметь любой из следующих типов (или быть коллекцией этих типов):</span><span class="sxs-lookup"><span data-stu-id="d25c4-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="d25c4-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="d25c4-749">тип сущности</span><span class="sxs-lookup"><span data-stu-id="d25c4-749">entity type</span></span> <br/> <span data-ttu-id="d25c4-750">сложный тип</span><span class="sxs-lookup"><span data-stu-id="d25c4-750">complex type</span></span> <br/> <span data-ttu-id="d25c4-751">тип строки</span><span class="sxs-lookup"><span data-stu-id="d25c4-751">row type</span></span> <br/> <span data-ttu-id="d25c4-752">ссылочный тип</span><span class="sxs-lookup"><span data-stu-id="d25c4-752">reference type</span></span>                             |
| <span data-ttu-id="d25c4-753">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-753">**Nullable**</span></span>     | <span data-ttu-id="d25c4-754">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-754">No</span></span>          | <span data-ttu-id="d25c4-755">**Значение true,** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь **null** значение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="d25c4-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="d25c4-756">**DefaultValue**</span></span> | <span data-ttu-id="d25c4-757">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-757">No</span></span>          | <span data-ttu-id="d25c4-758">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d25c4-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d25c4-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-759">**MaxLength**</span></span>    | <span data-ttu-id="d25c4-760">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-760">No</span></span>          | <span data-ttu-id="d25c4-761">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-762">**FixedLength**</span></span>  | <span data-ttu-id="d25c4-763">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-763">No</span></span>          | <span data-ttu-id="d25c4-764">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="d25c4-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d25c4-765">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-765">**Precision**</span></span>    | <span data-ttu-id="d25c4-766">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-766">No</span></span>          | <span data-ttu-id="d25c4-767">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d25c4-768">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-768">**Scale**</span></span>        | <span data-ttu-id="d25c4-769">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-769">No</span></span>          | <span data-ttu-id="d25c4-770">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d25c4-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-771">**SRID**</span></span>         | <span data-ttu-id="d25c4-772">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-772">No</span></span>          | <span data-ttu-id="d25c4-773">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-774">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d25c4-775">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d25c4-776">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-776">**Unicode**</span></span>      | <span data-ttu-id="d25c4-777">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-777">No</span></span>          | <span data-ttu-id="d25c4-778">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d25c4-779">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-779">**Collation**</span></span>    | <span data-ttu-id="d25c4-780">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-780">No</span></span>          | <span data-ttu-id="d25c4-781">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d25c4-782">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="d25c4-783">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-784">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-785">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-785">Example</span></span>

<span data-ttu-id="d25c4-786">В следующем примере показан **функция** элемент, который использует один **параметр** дочернего элемента для определения параметра функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="d25c4-787">Элемент Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="d25c4-788">**Участника** элемента на языке определения концептуальной схемы (CSDL) является дочерним элементом для элемента ReferentialConstraint, который определяет основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="d25c4-789">Объект **ReferentialConstraint** элемент определяет функциональные возможности, схожие с возможностями ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d25c4-790">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d25c4-791">Тип сущности, на который приведена ссылка называется *основного конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d25c4-792">Тип сущности, который ссылается на основном конечном элементе называется *зависимого конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="d25c4-793">**PropertyRef** элементы, используемые для указания, какие ключи ссылается зависимый конечный.</span><span class="sxs-lookup"><span data-stu-id="d25c4-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="d25c4-794">**Участника** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-795">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d25c4-796">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-797">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-797">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-798">В следующей таблице описаны атрибуты, которые могут применяться к **участника** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="d25c4-799">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-799">Attribute Name</span></span> | <span data-ttu-id="d25c4-800">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-800">Is Required</span></span> | <span data-ttu-id="d25c4-801">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="d25c4-802">**Роль**</span><span class="sxs-lookup"><span data-stu-id="d25c4-802">**Role**</span></span>       | <span data-ttu-id="d25c4-803">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-803">Yes</span></span>         | <span data-ttu-id="d25c4-804">Имя типа сущности в основном конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-805">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **участника** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="d25c4-806">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-807">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-808">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-808">Example</span></span>

<span data-ttu-id="d25c4-809">В следующем примере показан **ReferentialConstraint** элемент, который является частью определения **PublishedBy** ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="d25c4-810">**Идентификатор** свойство **издателя** тип сущности составляет основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="d25c4-811">Элемент Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-811">Property Element (CSDL)</span></span>

<span data-ttu-id="d25c4-812">**Свойство** элемент на языке определения концептуальной схемы (CSDL) может быть дочерний элемент EntityType, ComplexType-элемент или элемент RowType.</span><span class="sxs-lookup"><span data-stu-id="d25c4-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="d25c4-813">Применение элементов EntityType и ComplexType</span><span class="sxs-lookup"><span data-stu-id="d25c4-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="d25c4-814">**Свойство** элементов (как дочерние элементы **EntityType** или **ComplexType** элементы) определяют форму и характеристики данных, который будет содержать экземпляр типа сущности или сложного типа .</span><span class="sxs-lookup"><span data-stu-id="d25c4-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="d25c4-815">Свойства в концептуальной модели аналогичны свойствам, которые определены в классе.</span><span class="sxs-lookup"><span data-stu-id="d25c4-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="d25c4-816">По такому же принципу, как свойства, относящиеся к классу, определяют форму класса и несут информацию об объектах, свойства в концептуальной модели определяют форму типа сущности и несут информацию об экземплярах типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="d25c4-817">**Свойство** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-818">Элемент Documentation (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-819">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="d25c4-820">Следующие аспекты, которые могут применяться к **свойство** элемент: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **точности**, **масштабирования**, **Юникода**, **параметры сортировки**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="d25c4-821">Аспекты представляют собой атрибуты XML, которые предоставляют сведения о том, как значения свойств хранятся в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-822">Аспекты может применяться только к свойствам этого типа **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-823">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-823">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-824">В следующей таблице описаны атрибуты, которые могут применяться к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="d25c4-825">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-825">Attribute Name</span></span>                                                         | <span data-ttu-id="d25c4-826">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-826">Is Required</span></span> | <span data-ttu-id="d25c4-827">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-828">**Name**</span></span>                                                               | <span data-ttu-id="d25c4-829">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-829">Yes</span></span>         | <span data-ttu-id="d25c4-830">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-831">**Type**</span></span>                                                               | <span data-ttu-id="d25c4-832">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-832">Yes</span></span>         | <span data-ttu-id="d25c4-833">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-833">The type of the property value.</span></span> <span data-ttu-id="d25c4-834">Тип значения свойства должен быть **EDMSimpleType** или сложного типа (указано в полное доменное имя), который находится в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="d25c4-835">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-835">**Nullable**</span></span>                                                           | <span data-ttu-id="d25c4-836">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-836">No</span></span>          | <span data-ttu-id="d25c4-837">**Значение true,** (значение по умолчанию) или <strong>False</strong> в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="d25c4-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="d25c4-838">> В языке CSDL v1 должен иметь свойство сложного типа `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="d25c4-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="d25c4-840">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-840">No</span></span>          | <span data-ttu-id="d25c4-841">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d25c4-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d25c4-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="d25c4-843">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-843">No</span></span>          | <span data-ttu-id="d25c4-844">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="d25c4-846">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-846">No</span></span>          | <span data-ttu-id="d25c4-847">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="d25c4-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d25c4-848">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-848">**Precision**</span></span>                                                          | <span data-ttu-id="d25c4-849">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-849">No</span></span>          | <span data-ttu-id="d25c4-850">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d25c4-851">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-851">**Scale**</span></span>                                                              | <span data-ttu-id="d25c4-852">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-852">No</span></span>          | <span data-ttu-id="d25c4-853">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d25c4-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-854">**SRID**</span></span>                                                               | <span data-ttu-id="d25c4-855">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-855">No</span></span>          | <span data-ttu-id="d25c4-856">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-857">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d25c4-858">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d25c4-859">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-859">**Unicode**</span></span>                                                            | <span data-ttu-id="d25c4-860">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-860">No</span></span>          | <span data-ttu-id="d25c4-861">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d25c4-862">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-862">**Collation**</span></span>                                                          | <span data-ttu-id="d25c4-863">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-863">No</span></span>          | <span data-ttu-id="d25c4-864">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="d25c4-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="d25c4-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="d25c4-866">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-866">No</span></span>          | <span data-ttu-id="d25c4-867">**Нет** (значение по умолчанию) или **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="d25c4-868">Если присвоено значение **Fixed**, значение свойства будет использоваться в проверках оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="d25c4-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="d25c4-869">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="d25c4-870">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-871">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-872">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-872">Example</span></span>

<span data-ttu-id="d25c4-873">В следующем примере показан **EntityType** элемент с тремя **свойство** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="d25c4-874">В следующем примере показан **ComplexType** элемент с пятью **свойство** элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="d25c4-875">Применение элемента RowType</span><span class="sxs-lookup"><span data-stu-id="d25c4-875">RowType Element Application</span></span>

<span data-ttu-id="d25c4-876">**Свойство** элементы (будучи дочерними по отношению **RowType** элемент) определяют форму и характеристики данных, который может быть передан или возвращаемых определяемой моделью функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="d25c4-877">**Свойство** элемент может иметь только один из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="d25c4-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d25c4-878">CollectionType</span></span>
-   <span data-ttu-id="d25c4-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d25c4-879">ReferenceType</span></span>
-   <span data-ttu-id="d25c4-880">RowType</span><span class="sxs-lookup"><span data-stu-id="d25c4-880">RowType</span></span>

<span data-ttu-id="d25c4-881">**Свойство** элемент может иметь любое число дочерних элементов заметки.</span><span class="sxs-lookup"><span data-stu-id="d25c4-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-882">Элементы annotation допускаются только в том случае, в языке CSDL версии 2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d25c4-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-883">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-883">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-884">В следующей таблице описаны атрибуты, которые могут применяться к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="d25c4-885">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-885">Attribute Name</span></span>                                                     | <span data-ttu-id="d25c4-886">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-886">Is Required</span></span> | <span data-ttu-id="d25c4-887">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-888">**Name**</span></span>                                                           | <span data-ttu-id="d25c4-889">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-889">Yes</span></span>         | <span data-ttu-id="d25c4-890">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-891">**Type**</span></span>                                                           | <span data-ttu-id="d25c4-892">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-892">Yes</span></span>         | <span data-ttu-id="d25c4-893">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-894">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-894">**Nullable**</span></span>                                                       | <span data-ttu-id="d25c4-895">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-895">No</span></span>          | <span data-ttu-id="d25c4-896">**Значение true,** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="d25c4-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="d25c4-897">> В языке CSDL v1 должен иметь свойство сложного типа `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="d25c4-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="d25c4-899">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-899">No</span></span>          | <span data-ttu-id="d25c4-900">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d25c4-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d25c4-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="d25c4-902">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-902">No</span></span>          | <span data-ttu-id="d25c4-903">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="d25c4-905">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-905">No</span></span>          | <span data-ttu-id="d25c4-906">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="d25c4-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d25c4-907">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-907">**Precision**</span></span>                                                      | <span data-ttu-id="d25c4-908">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-908">No</span></span>          | <span data-ttu-id="d25c4-909">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d25c4-910">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-910">**Scale**</span></span>                                                          | <span data-ttu-id="d25c4-911">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-911">No</span></span>          | <span data-ttu-id="d25c4-912">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d25c4-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-913">**SRID**</span></span>                                                           | <span data-ttu-id="d25c4-914">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-914">No</span></span>          | <span data-ttu-id="d25c4-915">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-916">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d25c4-917">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d25c4-918">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-918">**Unicode**</span></span>                                                        | <span data-ttu-id="d25c4-919">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-919">No</span></span>          | <span data-ttu-id="d25c4-920">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d25c4-921">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-921">**Collation**</span></span>                                                      | <span data-ttu-id="d25c4-922">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-922">No</span></span>          | <span data-ttu-id="d25c4-923">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d25c4-924">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="d25c4-925">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-926">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d25c4-927">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-927">Example</span></span>

<span data-ttu-id="d25c4-928">В следующем примере показан **свойство** элементы, используемые для определения вида тип возвращаемого значения определяемой моделью функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="d25c4-929">Элемент PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="d25c4-930">**PropertyRef** элемент на языке определения концептуальной схемы (CSDL) ссылается на свойство типа сущности, чтобы указать, что свойство будет выступать в одной из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="d25c4-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="d25c4-931">Часть ключа сущности (является свойством или набором свойств типа сущности, которые определяют идентификатор).</span><span class="sxs-lookup"><span data-stu-id="d25c4-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="d25c4-932">Один или несколько **PropertyRef** элементы можно использовать для определения ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="d25c4-933">Зависимый или основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="d25c4-934">**PropertyRef** элемент может содержать только элементы annotation (ноль и более) как дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-935">Элементы annotation допускаются только в том случае, в языке CSDL версии 2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d25c4-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-936">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-936">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-937">В следующей таблице описаны атрибуты, которые могут применяться к **PropertyRef** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="d25c4-938">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-938">Attribute Name</span></span> | <span data-ttu-id="d25c4-939">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-939">Is Required</span></span> | <span data-ttu-id="d25c4-940">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="d25c4-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="d25c4-941">**Name**</span></span>       | <span data-ttu-id="d25c4-942">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-942">Yes</span></span>         | <span data-ttu-id="d25c4-943">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="d25c4-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-944">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **PropertyRef** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="d25c4-945">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-946">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-947">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-947">Example</span></span>

<span data-ttu-id="d25c4-948">В приведенном ниже примере определяется тип сущности (**книги**).</span><span class="sxs-lookup"><span data-stu-id="d25c4-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="d25c4-949">Ключ сущности определяется ссылкой на **ISBN** свойство типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="d25c4-950">В следующем примере два **PropertyRef** элементов используются для указания, что два свойства (**идентификатор** и **PublisherId**) являются основным и зависимым окончания ссылочном ограничение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="d25c4-951">Элемент ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-952">**ReferenceType** элемент на языке определения концептуальной схемы (CSDL) задает ссылку на тип сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="d25c4-953">**ReferenceType** элемент может быть дочерним элементом следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="d25c4-954">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="d25c4-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="d25c4-955">Параметр</span><span class="sxs-lookup"><span data-stu-id="d25c4-955">Parameter</span></span>
-   <span data-ttu-id="d25c4-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d25c4-956">CollectionType</span></span>

<span data-ttu-id="d25c4-957">**ReferenceType** элемент используется при задании параметра или возвращаемого типа для функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="d25c4-958">Объект **ReferenceType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-959">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-960">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-961">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-961">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-962">В следующей таблице описаны атрибуты, которые могут применяться к **ReferenceType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="d25c4-963">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-963">Attribute Name</span></span> | <span data-ttu-id="d25c4-964">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-964">Is Required</span></span> | <span data-ttu-id="d25c4-965">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="d25c4-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-966">**Type**</span></span>       | <span data-ttu-id="d25c4-967">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-967">Yes</span></span>         | <span data-ttu-id="d25c4-968">Имя типа сущности, на который делается ссылка.</span><span class="sxs-lookup"><span data-stu-id="d25c4-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-969">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ReferenceType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="d25c4-970">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-971">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-972">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-972">Example</span></span>

<span data-ttu-id="d25c4-973">В следующем примере показан **ReferenceType** элемент, используемый в качестве дочернего элемента **параметр** элемент в определяемой моделью функции, которая принимает ссылку на **Person** сущности Тип:</span><span class="sxs-lookup"><span data-stu-id="d25c4-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="d25c4-974">В следующем примере показан **ReferenceType** элемент, используемый в качестве дочернего элемента **ReturnType** элемент (Function) в определяемой моделью функции, которая возвращает ссылку на **Person**типа сущности:</span><span class="sxs-lookup"><span data-stu-id="d25c4-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="d25c4-975">Элемент ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="d25c4-976">Объект **ReferentialConstraint** элемент на языке определения концептуальной схемы (CSDL) определяет функциональные возможности, схожие с возможностями ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d25c4-977">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d25c4-978">Тип сущности, на который приведена ссылка называется *основного конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d25c4-979">Тип сущности, который ссылается на основном конечном элементе называется *зависимого конца* ограничения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="d25c4-980">Если внешний ключ, который предоставляется в одном типе сущности, ссылается на свойство другого типа сущности, **ReferentialConstraint** элемент определяет ассоциацию между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="d25c4-981">Так как **ReferentialConstraint** элемент предоставляет связанные сведения о том, как два типа сущностей, соответствующий элемент AssociationSetMapping не является необходимым в язык определения сопоставлений (MSL).</span><span class="sxs-lookup"><span data-stu-id="d25c4-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="d25c4-982">Связь между двумя типами сущностей, у которых нет представленных внешних ключей должна иметь соответствующий **AssociationSetMapping** элемент для сопоставления сведений об ассоциации с источником данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="d25c4-983">Если внешний ключ недоступен для типа сущности **ReferentialConstraint** элемент можно определить только ключ к первичному ограничение первичного ключа между типом сущности и другого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="d25c4-984">Объект **ReferentialConstraint** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-985">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-986">Участник (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="d25c4-987">Зависимые (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="d25c4-988">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-989">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-989">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-990">**ReferentialConstraint** элемент может иметь любое количество атрибутов annotation (настраиваемых атрибутов XML).</span><span class="sxs-lookup"><span data-stu-id="d25c4-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="d25c4-991">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-992">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-993">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-993">Example</span></span>

<span data-ttu-id="d25c4-994">В следующем примере показан **ReferentialConstraint** элемента, используемого в качестве части определения **PublishedBy** ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d25c4-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="d25c4-995">Элемент ReturnType (Function) (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="d25c4-996">**ReturnType** элемент (функция) в языке определения концептуальных схем (CSDL) определяет тип возвращаемого значения для функции, которая определяется в элементе функции.</span><span class="sxs-lookup"><span data-stu-id="d25c4-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="d25c4-997">Тип возвращаемого значения также можно задать с помощью функции **ReturnType** атрибута.</span><span class="sxs-lookup"><span data-stu-id="d25c4-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="d25c4-998">Возвращаемым типом может быть любой **EdmSimpleType**, тип сущности, сложный тип, тип строки, ссылочный тип или коллекцию из одного из этих типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="d25c4-999">Тип возвращаемого значения функции, которые можно указать с помощью **тип** атрибут **ReturnType** элемент (Function), или с помощью одного из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="d25c4-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1000">CollectionType</span></span>
-   <span data-ttu-id="d25c4-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1001">ReferenceType</span></span>
-   <span data-ttu-id="d25c4-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-1003">Модель не пройдет проверку, если указан возвращаемый тип с обоими функции **тип** атрибут **ReturnType** (функция) и с помощью одного из дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1004">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1004">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1005">В следующей таблице описаны атрибуты, которые могут применяться к **ReturnType** элемент (Function).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="d25c4-1006">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-1006">Attribute Name</span></span> | <span data-ttu-id="d25c4-1007">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-1007">Is Required</span></span> | <span data-ttu-id="d25c4-1008">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="d25c4-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1009">**ReturnType**</span></span> | <span data-ttu-id="d25c4-1010">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1010">No</span></span>          | <span data-ttu-id="d25c4-1011">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-1012">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ReturnType** элемент (Function).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="d25c4-1013">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1014">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-1015">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1015">Example</span></span>

<span data-ttu-id="d25c4-1016">В следующем примере используется **функция** для определения функции, которая возвращает количество книги кондиционирования в годах печати.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="d25c4-1017">Обратите внимание, что тип возвращаемого значения определяется **тип** атрибут **ReturnType** элемент (Function).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="d25c4-1018">Элемент ReturnType (FunctionImport) (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="d25c4-1019">**ReturnType** элемент (FunctionImport) на языке определения концептуальной схемы (CSDL) определяет тип возвращаемого значения для функции, которая определена в элемент FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="d25c4-1020">Тип возвращаемого значения также можно задать с помощью функции **ReturnType** атрибута.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="d25c4-1021">Возвращаемым типом может быть любой коллекции типа сущности, сложный тип или **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="d25c4-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="d25c4-1022">Тип возвращаемого значения функции указан с **тип** атрибут **ReturnType** элемент (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1023">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1023">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1024">В следующей таблице описаны атрибуты, которые могут применяться к **ReturnType** элемент (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="d25c4-1025">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-1025">Attribute Name</span></span> | <span data-ttu-id="d25c4-1026">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-1026">Is Required</span></span> | <span data-ttu-id="d25c4-1027">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1028">**Type**</span></span>       | <span data-ttu-id="d25c4-1029">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1029">No</span></span>          | <span data-ttu-id="d25c4-1030">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1030">The type that the function returns.</span></span> <span data-ttu-id="d25c4-1031">Значение должно быть коллекция ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="d25c4-1032">**Набор EntitySet**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1032">**EntitySet**</span></span>  | <span data-ttu-id="d25c4-1033">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1033">No</span></span>          | <span data-ttu-id="d25c4-1034">Если функция возвращает коллекцию сущностей типов, значение **EntitySet** должен представлять собой набор сущностей, которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="d25c4-1035">В противном случае **EntitySet** атрибут не должен использоваться.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-1036">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ReturnType** элемент (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="d25c4-1037">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1038">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-1039">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1039">Example</span></span>

<span data-ttu-id="d25c4-1040">В следующем примере используется **FunctionImport** , возвращающий книги и издателей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="d25c4-1041">Обратите внимание, что функция возвращает два результирующих набора и, следовательно, две **ReturnType** указаны элементы (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="d25c4-1042">Элемент RowType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="d25c4-1043">Объект **RowType** элемента на языке определения концептуальной схемы (CSDL) определяет безымянную структуру в качестве параметра или тип возвращаемого значения для функции, определенной в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="d25c4-1044">Объект **RowType** элемент может быть дочерним элементом следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="d25c4-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1045">CollectionType</span></span>
-   <span data-ttu-id="d25c4-1046">Параметр</span><span class="sxs-lookup"><span data-stu-id="d25c4-1046">Parameter</span></span>
-   <span data-ttu-id="d25c4-1047">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1047">ReturnType (Function)</span></span>

<span data-ttu-id="d25c4-1048">Объект **RowType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-1049">Свойство (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1049">Property (one or more)</span></span>
-   <span data-ttu-id="d25c4-1050">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1051">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1051">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1052">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **RowType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="d25c4-1053">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1054">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-1055">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1055">Example</span></span>

<span data-ttu-id="d25c4-1056">В следующем примере показано определяемой моделью функции, который использует **CollectionType** элемента, чтобы указать, что функция возвращает коллекцию строк (как указано в **RowType** элемент).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="d25c4-1057">Элемент Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="d25c4-1058">**Схемы** элемент является корневым элементом определения концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="d25c4-1059">Он содержит определения объектов, функций и контейнеров, из которых состоит концептуальная модель.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="d25c4-1060">**Схемы** элемент может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="d25c4-1061">Using</span><span class="sxs-lookup"><span data-stu-id="d25c4-1061">Using</span></span>
-   <span data-ttu-id="d25c4-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="d25c4-1062">EntityContainer</span></span>
-   <span data-ttu-id="d25c4-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1063">EntityType</span></span>
-   <span data-ttu-id="d25c4-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1064">EnumType</span></span>
-   <span data-ttu-id="d25c4-1065">Ассоциация</span><span class="sxs-lookup"><span data-stu-id="d25c4-1065">Association</span></span>
-   <span data-ttu-id="d25c4-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1066">ComplexType</span></span>
-   <span data-ttu-id="d25c4-1067">Функция</span><span class="sxs-lookup"><span data-stu-id="d25c4-1067">Function</span></span>

<span data-ttu-id="d25c4-1068">Объект **схемы** элемент может содержать ноль или один элемент заметки.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-1069">**Функция** элементов и элементы annotation допускаются только в версии языка CSDL 2 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="d25c4-1070">**Схемы** элемент использует **пространства имен** атрибут для определения пространства имен для типа сущности, сложный тип и объектов ассоциации в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="d25c4-1071">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="d25c4-1072">Пространства имен может быть несколько **схемы** элементов и несколько файлов с расширением CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="d25c4-1073">Пространство имен концептуальной модели отличается от пространства имен XML элемента **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="d25c4-1074">Пространство имен концептуальной модели (как определено **пространства имен** атрибут) — это логический контейнер для типов сущностей, сложные типы и типы ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="d25c4-1075">Пространство имен XML (обозначается **xmlns** атрибут) из **схемы** элемент является пространство имен по умолчанию для дочерних элементов и атрибутов из **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="d25c4-1076">Пространства имен XML вида http://schemas.microsoft.com/ado/YYYY/MM/edm (где YYYY и MM представляют год и месяц соответственно) зарезервированы для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1077">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1078">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1078">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1079">В следующей таблице описываются атрибуты могут применяться к **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="d25c4-1080">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-1080">Attribute Name</span></span> | <span data-ttu-id="d25c4-1081">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-1081">Is Required</span></span> | <span data-ttu-id="d25c4-1082">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-1083">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1083">**Namespace**</span></span>  | <span data-ttu-id="d25c4-1084">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1084">Yes</span></span>         | <span data-ttu-id="d25c4-1085">Пространство имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="d25c4-1086">Значение **пространства имен** атрибут используется, чтобы сформировать полное имя типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="d25c4-1087">Например если **EntityType** с именем *клиента* находится в пространстве имен Simple.Example.Model, а затем полностью квалифицированное имя **EntityType** является SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="d25c4-1088">Следующие строки не может использоваться как значение для **пространства имен** атрибут: **системы**, **временных**, или **Edm**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="d25c4-1089">Значение для **пространства имен** атрибут не может быть таким же, как значение для **пространства имен** атрибут в элементе SSDL-схеме.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="d25c4-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1090">**Alias**</span></span>      | <span data-ttu-id="d25c4-1091">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1091">No</span></span>          | <span data-ttu-id="d25c4-1092">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="d25c4-1093">Например если **EntityType** с именем *клиента* находится в пространстве имен Simple.Example.Model, а значение **псевдоним** атрибут *модели*, то в качестве полное доменное имя можно указать Model.Customer **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="d25c4-1094">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="d25c4-1095">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1096">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-1097">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1097">Example</span></span>

<span data-ttu-id="d25c4-1098">В следующем примере показан **схемы** элемент, содержащий **EntityContainer** элемент, двух **EntityType** элементов и один **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="d25c4-1099">Элемент TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="d25c4-1100">**TypeRef** элемент на языке определения концептуальной схемы (CSDL) предоставляет ссылку на существующий именованный тип.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="d25c4-1101">**TypeRef** элемент может быть дочерним элементом элемента CollectionType, который используется для указания, что функция имеет коллекцию в качестве параметра или возвращаемого типа.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="d25c4-1102">Объект **TypeRef** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="d25c4-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d25c4-1103">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d25c4-1104">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1105">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1105">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1106">В следующей таблице описаны атрибуты, которые могут применяться к **TypeRef** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="d25c4-1107">Обратите внимание, что **DefaultValue**, **MaxLength**, **FixedLength**, **точности**, **масштабирования**,  **Юникод**, и **параметры сортировки** атрибуты применимы только к **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="d25c4-1108">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="d25c4-1109">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-1109">Is Required</span></span> | <span data-ttu-id="d25c4-1110">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1111">**Type**</span></span>                                                           | <span data-ttu-id="d25c4-1112">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1112">No</span></span>          | <span data-ttu-id="d25c4-1113">Имя типа, на который дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="d25c4-1114">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="d25c4-1115">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1115">No</span></span>          | <span data-ttu-id="d25c4-1116">**Значение true,** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="d25c4-1117">> В языке CSDL v1 должен иметь свойство сложного типа `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d25c4-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="d25c4-1119">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1119">No</span></span>          | <span data-ttu-id="d25c4-1120">Значение свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d25c4-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="d25c4-1122">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1122">No</span></span>          | <span data-ttu-id="d25c4-1123">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d25c4-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="d25c4-1125">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1125">No</span></span>          | <span data-ttu-id="d25c4-1126">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d25c4-1127">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1127">**Precision**</span></span>                                                      | <span data-ttu-id="d25c4-1128">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1128">No</span></span>          | <span data-ttu-id="d25c4-1129">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d25c4-1130">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1130">**Scale**</span></span>                                                          | <span data-ttu-id="d25c4-1131">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1131">No</span></span>          | <span data-ttu-id="d25c4-1132">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d25c4-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1133">**SRID**</span></span>                                                           | <span data-ttu-id="d25c4-1134">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1134">No</span></span>          | <span data-ttu-id="d25c4-1135">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d25c4-1136">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d25c4-1137">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d25c4-1138">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="d25c4-1139">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1139">No</span></span>          | <span data-ttu-id="d25c4-1140">**Значение true,** или **False** в зависимости от того, будет ли значение свойства храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d25c4-1141">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1141">**Collation**</span></span>                                                      | <span data-ttu-id="d25c4-1142">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1142">No</span></span>          | <span data-ttu-id="d25c4-1143">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d25c4-1144">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **CollectionType** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d25c4-1145">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1146">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-1147">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1147">Example</span></span>

<span data-ttu-id="d25c4-1148">В следующем примере показано определяемой моделью функции, который использует **TypeRef** элемент (как дочерний **CollectionType** элемент) для указания, что функция принимает коллекцию  **Отдел** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="d25c4-1149">Элемент Using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="d25c4-1150">**Using** элемента на языке определения концептуальной схемы (CSDL) импортирует содержимое концептуальной модели, который существует в другом пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="d25c4-1151">Установив значение **пространства имен** атрибут, можно ссылаться на типы сущностей, сложные типы и типы ассоциаций, которые определены в другой концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="d25c4-1152">Более одного **Using** элемент может быть дочерним элементом **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-1153">**Using** элемента на языке CSDL не работает абсолютно идентично **с помощью** инструкции на языке программирования.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="d25c4-1154">Путем импорта пространства имен с **с помощью** инструкции на языке программирования, вы не влияют на объекты в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="d25c4-1155">В языке CSDL импортированное пространство имен может содержать тип сущности, производный от типа сущности в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="d25c4-1156">Это может влиять на наборы сущностей, объявленные в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="d25c4-1157">**Using** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="d25c4-1158">Документация (ноль или один допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d25c4-1159">Элементы annotation (ноль или более допустимое количество элементов)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d25c4-1160">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1160">Applicable Attributes</span></span>

<span data-ttu-id="d25c4-1161">В следующей таблице описываются атрибуты могут применяться к **Using** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="d25c4-1162">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="d25c4-1162">Attribute Name</span></span> | <span data-ttu-id="d25c4-1163">Необходимо</span><span class="sxs-lookup"><span data-stu-id="d25c4-1163">Is Required</span></span> | <span data-ttu-id="d25c4-1164">Значение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-1165">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1165">**Namespace**</span></span>  | <span data-ttu-id="d25c4-1166">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1166">Yes</span></span>         | <span data-ttu-id="d25c4-1167">Имя импортируемого пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="d25c4-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1168">**Alias**</span></span>      | <span data-ttu-id="d25c4-1169">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1169">Yes</span></span>         | <span data-ttu-id="d25c4-1170">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="d25c4-1171">Хотя этот атрибут является обязательным, необязательно, чтобы он использовался вместо имени пространства имен при указании имен объектов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d25c4-1172">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **Using** элемент.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="d25c4-1173">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d25c4-1174">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d25c4-1175">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1175">Example</span></span>

<span data-ttu-id="d25c4-1176">В следующем примере демонстрируется **Using** элемента, используемого для импорта пространства имен, определенный в другом.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="d25c4-1177">Обратите внимание, что пространство имен для **схемы** является элемент, показанный `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="d25c4-1178">`Address` Свойство `Publisher` **EntityType** — это сложный тип, который определен в `ExtendedBooksModel` пространства имен (импортированы с **Using** элемент).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="d25c4-1179">Атрибуты примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="d25c4-1180">Атрибуты annotation в языке CSDL представляют собой настраиваемые атрибуты XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="d25c4-1181">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="d25c4-1182">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="d25c4-1183">К данному конкретному элементу языка CSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="d25c4-1184">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="d25c4-1185">Атрибуты annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="d25c4-1186">Метаданные, содержащиеся в элементах annotation может осуществляться во время выполнения с помощью классов в пространстве имен System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-1187">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1187">Example</span></span>

<span data-ttu-id="d25c4-1188">В следующем примере показан **EntityType** элемент с атрибутом annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="d25c4-1189">В следующем примере показано применение элемента annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="d25c4-1190">Следующий код извлекает метаданные из атрибута annotation и выводит их на консоль:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="d25c4-1191">Вышеуказанный код предполагает, что файл `School.csdl` находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="d25c4-1192">Элементы примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="d25c4-1193">Элементы annotation в языке CSDL представляют собой настраиваемые элементы XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="d25c4-1194">Элементы annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="d25c4-1195">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="d25c4-1196">У данного элемента языка CSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="d25c4-1197">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="d25c4-1198">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="d25c4-1199">Элементы annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="d25c4-1200">Начиная с .NET Framework версии 4, метаданные, содержащиеся в элементах annotation может осуществляться во время выполнения с помощью классов в пространстве имен System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-1201">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1201">Example</span></span>

<span data-ttu-id="d25c4-1202">В следующем примере показан **EntityType** элемент с элементом annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="d25c4-1203">В следующем примере показано применение атрибута annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="d25c4-1204">Следующий код извлекает метаданные из элемента annotation и выводит их на консоль.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="d25c4-1205">Вышеприведенный код предполагает, что файл School.csdl находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="d25c4-1206">Типы концептуальной модели (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="d25c4-1207">Язык определения концептуальной схемы (CSDL) поддерживает набор абстрактных примитивных типов данных, называется **EDMSimpleTypes**, которые определяют свойства в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="d25c4-1208">**EDMSimpleTypes** являются посредниками для примитивных типов данных, которые поддерживаются в среде хранения или размещения.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="d25c4-1209">В приведенной ниже таблице представлены примитивные типы данных, поддерживаемые CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="d25c4-1210">В таблице также перечислены аспекты, которые могут применяться к каждому **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="d25c4-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="d25c4-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="d25c4-1212">Описание:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1212">Description</span></span>                                                | <span data-ttu-id="d25c4-1213">Применимые аспекты</span><span class="sxs-lookup"><span data-stu-id="d25c4-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="d25c4-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="d25c4-1215">Содержит двоичные данные.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="d25c4-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="d25c4-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="d25c4-1218">Содержит значение **true** или **false**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="d25c4-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="d25c4-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="d25c4-1221">Содержит 8-битное целое значение без знака.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="d25c4-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="d25c4-1224">Представляет дату и время.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="d25c4-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="d25c4-1227">Возвращает дату и время в виде смещения в минутах от времени GMT.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="d25c4-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="d25c4-1230">Содержит точное числовое значение с заданной точностью и масштабом.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="d25c4-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="d25c4-1233">Содержит число с плавающей запятой с точностью до 15 цифр</span><span class="sxs-lookup"><span data-stu-id="d25c4-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="d25c4-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="d25c4-1236">Содержит число с плавающей запятой с точностью до 7 знаков.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="d25c4-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="d25c4-1239">Содержит уникальный 16-битный идентификатор.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="d25c4-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="d25c4-1242">Содержит 16-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="d25c4-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="d25c4-1245">Содержит 32-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="d25c4-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="d25c4-1248">Содержит 64-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="d25c4-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="d25c4-1251">Содержит 8-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="d25c4-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1253">**Edm.String**</span></span>                   | <span data-ttu-id="d25c4-1254">Содержит символьные данные.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1254">Contains character data.</span></span>                                   | <span data-ttu-id="d25c4-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="d25c4-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="d25c4-1257">Содержит время дня.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="d25c4-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="d25c4-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d25c4-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="d25c4-1260">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="d25c4-1262">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="d25c4-1264">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="d25c4-1266">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="d25c4-1268">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="d25c4-1270">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="d25c4-1272">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="d25c4-1274">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="d25c4-1276">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="d25c4-1278">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="d25c4-1280">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="d25c4-1282">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="d25c4-1284">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="d25c4-1286">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="d25c4-1288">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d25c4-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="d25c4-1290">Допускающие значение NULL, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="d25c4-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="d25c4-1291">Области (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d25c4-1291">Facets (CSDL)</span></span>

<span data-ttu-id="d25c4-1292">Аспекты в языке CSDL представляют ограничения для свойств типов сущности и сложных типов.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="d25c4-1293">Аспекты выглядят как XML-атрибуты в следующих элементах CSDL:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="d25c4-1294">Свойство.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1294">Property</span></span>
-   <span data-ttu-id="d25c4-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="d25c4-1295">TypeRef</span></span>
-   <span data-ttu-id="d25c4-1296">Параметр</span><span class="sxs-lookup"><span data-stu-id="d25c4-1296">Parameter</span></span>

<span data-ttu-id="d25c4-1297">В следующей таблице описываются аспекты, поддерживаемые в CSDL.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="d25c4-1298">Все аспекты являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1298">All facets are optional.</span></span> <span data-ttu-id="d25c4-1299">Некоторые перечисленные ниже аспекты используются платформой Entity Framework, при создании базы данных из концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="d25c4-1300">Сведения о типах данных в концептуальной модели см. в разделе типов концептуальной модели (CSDL).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="d25c4-1301">Аспект</span><span class="sxs-lookup"><span data-stu-id="d25c4-1301">Facet</span></span>               | <span data-ttu-id="d25c4-1302">Описание</span><span class="sxs-lookup"><span data-stu-id="d25c4-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="d25c4-1303">Применение</span><span class="sxs-lookup"><span data-stu-id="d25c4-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="d25c4-1304">Используется для создания базы данных</span><span class="sxs-lookup"><span data-stu-id="d25c4-1304">Used for the database generation</span></span> | <span data-ttu-id="d25c4-1305">Используется средой выполнения</span><span class="sxs-lookup"><span data-stu-id="d25c4-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="d25c4-1306">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1306">**Collation**</span></span>       | <span data-ttu-id="d25c4-1307">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="d25c4-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d25c4-1309">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1309">Yes</span></span>                              | <span data-ttu-id="d25c4-1310">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1310">No</span></span>                  |
| <span data-ttu-id="d25c4-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="d25c4-1312">Указывает, что значение свойства должно использоваться в проверках оптимистического управления параллелизмом.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="d25c4-1313">Все **EDMSimpleType** свойства</span><span class="sxs-lookup"><span data-stu-id="d25c4-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d25c4-1314">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1314">No</span></span>                               | <span data-ttu-id="d25c4-1315">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1315">Yes</span></span>                 |
| <span data-ttu-id="d25c4-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1316">**Default**</span></span>         | <span data-ttu-id="d25c4-1317">Задает значение по умолчанию для свойства в случае, если при создании экземпляра не было задано значение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="d25c4-1318">Все **EDMSimpleType** свойства</span><span class="sxs-lookup"><span data-stu-id="d25c4-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d25c4-1319">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1319">Yes</span></span>                              | <span data-ttu-id="d25c4-1320">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1320">Yes</span></span>                 |
| <span data-ttu-id="d25c4-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1321">**FixedLength**</span></span>     | <span data-ttu-id="d25c4-1322">Указывает, может ли изменяться длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="d25c4-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d25c4-1324">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1324">Yes</span></span>                              | <span data-ttu-id="d25c4-1325">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1325">No</span></span>                  |
| <span data-ttu-id="d25c4-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1326">**MaxLength**</span></span>       | <span data-ttu-id="d25c4-1327">Указывает максимальную длину значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="d25c4-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d25c4-1329">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1329">Yes</span></span>                              | <span data-ttu-id="d25c4-1330">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1330">No</span></span>                  |
| <span data-ttu-id="d25c4-1331">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1331">**Nullable**</span></span>        | <span data-ttu-id="d25c4-1332">Указывает, может ли свойство иметь **null** значение.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="d25c4-1333">Все **EDMSimpleType** свойства</span><span class="sxs-lookup"><span data-stu-id="d25c4-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d25c4-1334">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1334">Yes</span></span>                              | <span data-ttu-id="d25c4-1335">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1335">Yes</span></span>                 |
| <span data-ttu-id="d25c4-1336">**Точность**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1336">**Precision**</span></span>       | <span data-ttu-id="d25c4-1337">Для свойств типа **десятичное**, количество десятичных разрядов, может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="d25c4-1338">Для свойств типа **время**, **DateTime**, и **DateTimeOffset**, указывает количество цифр дробной части секунд значения свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="d25c4-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="d25c4-1340">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1340">Yes</span></span>                              | <span data-ttu-id="d25c4-1341">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1341">No</span></span>                  |
| <span data-ttu-id="d25c4-1342">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1342">**Scale**</span></span>           | <span data-ttu-id="d25c4-1343">Задает число цифр справа от десятичной запятой в значении свойства.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="d25c4-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="d25c4-1345">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1345">Yes</span></span>                              | <span data-ttu-id="d25c4-1346">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1346">No</span></span>                  |
| <span data-ttu-id="d25c4-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1347">**SRID**</span></span>            | <span data-ttu-id="d25c4-1348">Указывает идентификатор системы пространственных ссылок системы.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="d25c4-1349">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d25c4-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="d25c4-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="d25c4-1351">Нет</span><span class="sxs-lookup"><span data-stu-id="d25c4-1351">No</span></span>                               | <span data-ttu-id="d25c4-1352">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1352">Yes</span></span>                 |
| <span data-ttu-id="d25c4-1353">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1353">**Unicode**</span></span>         | <span data-ttu-id="d25c4-1354">Указывает, будет ли значение свойства храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="d25c4-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="d25c4-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d25c4-1356">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1356">Yes</span></span>                              | <span data-ttu-id="d25c4-1357">Да</span><span class="sxs-lookup"><span data-stu-id="d25c4-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="d25c4-1358">При создании базы данных из концептуальной модели, мастер создания базы данных будет распознавать значение **StoreGeneratedPattern** атрибут **свойство** элемент, если он находится в следующие пространство имен: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="d25c4-1359">Поддерживаемые значения для атрибута **удостоверений** и **вычисленные**.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="d25c4-1360">Значение **удостоверений** создаст столбец базы данных со значением идентификатора, который создается в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="d25c4-1361">Значение **вычисленные** создаст столбец со значением, вычисляемым в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d25c4-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="d25c4-1362">Пример</span><span class="sxs-lookup"><span data-stu-id="d25c4-1362">Example</span></span>

<span data-ttu-id="d25c4-1363">В следующем примере показывается применение аспектов к свойствам типа сущности:</span><span class="sxs-lookup"><span data-stu-id="d25c4-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
