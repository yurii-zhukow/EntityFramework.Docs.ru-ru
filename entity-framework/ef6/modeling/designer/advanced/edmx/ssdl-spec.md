---
title: Спецификация языка SSDL — EF6
description: Спецификация языка SSDL в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
uid: ef6/modeling/designer/advanced/edmx/ssdl-spec
ms.openlocfilehash: ab50579649c2e1b19d113cd127e52be995516e27
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620596"
---
# <a name="ssdl-specification"></a><span data-ttu-id="3b8ca-103">Спецификация SSDL</span><span class="sxs-lookup"><span data-stu-id="3b8ca-103">SSDL Specification</span></span>
<span data-ttu-id="3b8ca-104">SSDL представляет собой язык на основе XML, на котором описывается модель хранения в приложениях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-104">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="3b8ca-105">В приложении Entity Framework метаданные модели хранения загружаются из SSDL-файла (написанного на языке SSDL) в экземпляр класса System. Data. Metadata. EDM. StoreItemCollection и доступны с помощью методов в классе System. Data. Metadata. EDM. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-105">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="3b8ca-106">Entity Framework использует метаданные модели хранения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-106">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="3b8ca-107">Entity Framework Designer (конструктор EF) сохраняет сведения о модели хранения в EDMX-файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-107">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="3b8ca-108">Во время сборки Entity Designer использует сведения в EDMX-файле, чтобы создать SSDL-файл, необходимый для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-108">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="3b8ca-109">Версии языка SSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-109">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="3b8ca-110">Версия на языке SSDL</span><span class="sxs-lookup"><span data-stu-id="3b8ca-110">SSDL Version</span></span> | <span data-ttu-id="3b8ca-111">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="3b8ca-111">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="3b8ca-112">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="3b8ca-112">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="3b8ca-113">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="3b8ca-113">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="3b8ca-114">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="3b8ca-114">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="3b8ca-115">Элемент Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-115">Association Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-116">Элемент **Association** в языке SSDL указывает столбцы таблицы, которые участвуют в ограничении внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-116">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="3b8ca-117">Два обязательных дочерних элемента End указывают таблицы в концах сопоставления и кратность для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-117">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="3b8ca-118">Дополнительный элемент ReferentialConstraint указывает основной и зависимый концы сопоставления и участвующие столбцы.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-118">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="3b8ca-119">Если элемент **ReferentialConstraint** отсутствует, для указания сопоставлений столбцов для ассоциации необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-119">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="3b8ca-120">Элемент **Association** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-120">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-121">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-121">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-122">End (ровно два)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-122">End (exactly two)</span></span>
-   <span data-ttu-id="3b8ca-123">ReferentialConstraint (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-123">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-124">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-124">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-125">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-125">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-126">В следующей таблице описаны атрибуты, которые можно применить к элементу **Association** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-126">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="3b8ca-127">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-127">Attribute Name</span></span> | <span data-ttu-id="3b8ca-128">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-128">Is Required</span></span> | <span data-ttu-id="3b8ca-129">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-129">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-130">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-130">**Name**</span></span>       | <span data-ttu-id="3b8ca-131">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-131">Yes</span></span>         | <span data-ttu-id="3b8ca-132">Имя соответствующего ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-132">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-133">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-133">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="3b8ca-134">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-134">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-135">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-135">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-136">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-136">Example</span></span>

<span data-ttu-id="3b8ca-137">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK \_ кустомерордерс** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-137">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="3b8ca-138">Элемент AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-138">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-139">Элемент **AssociationSet** в языке SSDL представляет ограничение внешнего ключа между двумя таблицами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-139">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="3b8ca-140">Столбцы таблицы, принимающие участие в ограничении внешнего ключа, указаны в элементе Association.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-140">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="3b8ca-141">Элемент **Association** , соответствующий заданному элементу **AssociationSet** , задается в атрибуте **Association** элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-141">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="3b8ca-142">Наборы ассоциаций SSDL сопоставляются с наборами ассоциаций CSDL с помощью элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-142">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="3b8ca-143">Однако если сопоставление CSDL для заданного набора ассоциаций CSDL определено с помощью элемента ReferentialConstraint, соответствующий элемент **AssociationSetMapping** не требуется.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-143">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="3b8ca-144">В этом случае, если имеется элемент **AssociationSetMapping** , определяемые им сопоставления будут переопределены элементом **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-144">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="3b8ca-145">Элемент **AssociationSet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-145">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-146">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-146">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-147">End (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-147">End (zero or two)</span></span>
-   <span data-ttu-id="3b8ca-148">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-148">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-149">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-149">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-150">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-150">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="3b8ca-151">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-151">Attribute Name</span></span>  | <span data-ttu-id="3b8ca-152">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-152">Is Required</span></span> | <span data-ttu-id="3b8ca-153">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-153">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-154">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-154">**Name**</span></span>        | <span data-ttu-id="3b8ca-155">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-155">Yes</span></span>         | <span data-ttu-id="3b8ca-156">Имя ограничения внешнего ключа, представленное набором ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-156">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="3b8ca-157">**Взаимосвязь**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-157">**Association**</span></span> | <span data-ttu-id="3b8ca-158">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-158">Yes</span></span>         | <span data-ttu-id="3b8ca-159">Имя ассоциации, определяющее столбцы, которые участвуют в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-159">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-160">К элементу **AssociationSet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-160">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="3b8ca-161">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-161">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-162">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-162">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-163">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-163">Example</span></span>

<span data-ttu-id="3b8ca-164">В следующем примере показан элемент **AssociationSet** , представляющий `FK_CustomerOrders` ограничение внешнего ключа в базовой базе данных:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-164">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="3b8ca-165">Элемент CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-165">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-166">Элемент **CollectionType** в языке SSDL указывает, что возвращаемый функцией тип является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-166">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="3b8ca-167">Элемент **CollectionType** является дочерним по отношению к элементу ReturnType.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-167">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="3b8ca-168">Тип коллекции задается с помощью дочернего элемента RowType:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-168">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="3b8ca-169">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-169">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3b8ca-170">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-170">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-171">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-171">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-172">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-172">Example</span></span>

<span data-ttu-id="3b8ca-173">В следующем примере показана функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-173">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="3b8ca-174">Элемент CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-174">CommandText Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-175">Элемент **CommandText** в языке SSDL является дочерним элементом элемента Function, который позволяет определить инструкцию SQL, выполняемую в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-175">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="3b8ca-176">Элемент **CommandText** позволяет добавлять функции, аналогичные хранимым процедурам в базе данных, но вы определяете элемент **CommandText** в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-176">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="3b8ca-177">Элемент **CommandText** не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-177">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="3b8ca-178">Тело элемента **CommandText** должно быть допустимой инструкцией SQL для базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-178">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="3b8ca-179">К элементу **CommandText** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-179">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-180">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-180">Example</span></span>

<span data-ttu-id="3b8ca-181">В следующем примере показан элемент **Function** с дочерним элементом **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-181">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="3b8ca-182">Предоставьте функцию **упдатепродуктинордер** как метод в контексте ObjectContext, импортировав его в концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-182">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="3b8ca-183">Элемент DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-183">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-184">Элемент **DefiningQuery** в языке SSDL позволяет выполнять инструкцию SQL непосредственно в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-184">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="3b8ca-185">Элемент **DefiningQuery** обычно используется как представление базы данных, но представление определяется в модели хранения, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-185">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="3b8ca-186">Представление, определенное в элементе **DefiningQuery** , может быть сопоставлено с типом сущности в концептуальной модели через элемент EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-186">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="3b8ca-187">Такие сопоставления предназначены только для чтения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-187">These mappings are read-only.</span></span>  

<span data-ttu-id="3b8ca-188">В следующем синтаксисе языка SSDL показано объявление **набора EntitySet** , за которым следует элемент **DefiningQuery** , содержащий запрос, используемый для получения представления.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-188">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="3b8ca-189">Хранимые процедуры можно использовать в Entity Framework для включения сценариев чтения и записи для представлений.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-189">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="3b8ca-190">В качестве базовой таблицы для получения данных и обработки изменений хранимыми процедурами можно использовать либо представление источника данных, либо представление Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-190"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="3b8ca-191">Элемент **DefiningQuery** можно использовать для назначения Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-191">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="3b8ca-192">Хотя SQL Server Compact 3,5 не поддерживает хранимые процедуры, можно реализовать аналогичные функции с помощью элемента **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-192">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="3b8ca-193">Этот элемент также может быть полезен при создании хранимых процедур для преодоления несоответствий между типами данных, используемых в языке программирования, и типами данных источника данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-193">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="3b8ca-194">Можно написать параметр **DefiningQuery** , который принимает определенный набор параметров, а затем вызывает хранимую процедуру с другим набором параметров, например хранимую процедуру, которая удаляет данные.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-194">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="3b8ca-195">Элемент Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-195">Dependent Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-196">**Зависимый** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, определяющего зависимый конец ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-196">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="3b8ca-197">**Зависимый** элемент указывает столбец (или столбцы) в таблице, ссылающейся на первичный ключевой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-197">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="3b8ca-198">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-198">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="3b8ca-199">Элемент Principal задает столбцы первичного ключа, на которые ссылаются столбцы, указанные в **зависимом** элементе.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-199">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="3b8ca-200">**Зависимый** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-200">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-201">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-201">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="3b8ca-202">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-202">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-203">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-203">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-204">В следующей таблице описаны атрибуты, которые можно применить к **зависимому** элементу.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-204">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="3b8ca-205">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-205">Attribute Name</span></span> | <span data-ttu-id="3b8ca-206">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-206">Is Required</span></span> | <span data-ttu-id="3b8ca-207">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-207">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-208">**Роль**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-208">**Role**</span></span>       | <span data-ttu-id="3b8ca-209">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-209">Yes</span></span>         | <span data-ttu-id="3b8ca-210">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей ссылающийся столбец.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-210">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-211">К **зависимому** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-211">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="3b8ca-212">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-212">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-213">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-213">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-214">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-214">Example</span></span>

<span data-ttu-id="3b8ca-215">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK \_ кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-215">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="3b8ca-216">**Зависимый** элемент указывает столбец **CustomerID** таблицы **Order** как зависимый конец ограничения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-216">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="3b8ca-217">Элемент Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-217">Documentation Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-218">Элемент **Documentation** в языке SSDL может использоваться для предоставления сведений об объекте, определенном в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-218">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="3b8ca-219">Элемент **Documentation** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-219">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-220">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-220">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="3b8ca-221">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-221">(zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-222">**Лонгдескриптион**: расширенное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-222">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="3b8ca-223">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-223">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-224">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-224">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-225">К элементу **Documentation** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-225">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="3b8ca-226">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-226">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-227">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-227">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-228">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-228">Example</span></span>

<span data-ttu-id="3b8ca-229">В следующем примере показан элемент **Documentation** в качестве дочернего элемента элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-229">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="3b8ca-230">Элемент End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-230">End Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-231">Элемент **End** в языке SSDL указывает таблицу и количество строк в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-231">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="3b8ca-232">Элемент **End** может быть дочерним по отношению к элементу Association или элементу AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-232">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="3b8ca-233">В каждом из этих случаев дочерние элементы и применимые атрибуты будут различными.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-233">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="3b8ca-234">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="3b8ca-234">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="3b8ca-235">Элемент **End** (в качестве дочернего элемента **ассоциации** ) указывает таблицу и число строк в конце ограничения внешнего ключа с атрибутами **тип** и **кратность** соответственно.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-235">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="3b8ca-236">Элементы ограничения внешнего ключа определяются как часть ассоциации SSDL. Ассоциация SSDL должна содержать ровно два элемента.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-236">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="3b8ca-237">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-237">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-238">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-238">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-239">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-239">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-240">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-240">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-241">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-241">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-242">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **Association** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-242">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="3b8ca-243">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-243">Attribute Name</span></span>   | <span data-ttu-id="3b8ca-244">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-244">Is Required</span></span> | <span data-ttu-id="3b8ca-245">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-245">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-246">**Тип**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-246">**Type**</span></span>         | <span data-ttu-id="3b8ca-247">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-247">Yes</span></span>         | <span data-ttu-id="3b8ca-248">Полное имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-248">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="3b8ca-249">**Роль**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-249">**Role**</span></span>         | <span data-ttu-id="3b8ca-250">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-250">No</span></span>          | <span data-ttu-id="3b8ca-251">Значение атрибута **Role** либо в основном, либо в зависимом элементе соответствующего элемента ReferentialConstraint (если используется).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-251">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="3b8ca-252">**Кратность**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-252">**Multiplicity**</span></span> | <span data-ttu-id="3b8ca-253">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-253">Yes</span></span>         | <span data-ttu-id="3b8ca-254">**1**, **0.. 1**или в **\*** зависимости от количества строк, которые могут находиться в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-254">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="3b8ca-255">значение **1** указывает, что в конце ограничения внешнего ключа существует ровно одна строка.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-255">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="3b8ca-256">**0.. 1** указывает, что в окончании ограничения внешнего ключа находится ноль или одна строка.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-256">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="3b8ca-257">**\*** Указывает, что в окончании ограничения внешнего ключа существует ноль, одна или несколько строк.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-257">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-258">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-258">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3b8ca-259">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-259">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-260">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-260">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="3b8ca-261">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-261">Example</span></span>

<span data-ttu-id="3b8ca-262">В следующем примере показан элемент **Association** , определяющий ограничение внешнего ключа **FK \_ кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-262">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="3b8ca-263">Значения **кратности** , заданные для каждого **конечного** элемента, указывают на то, что многие строки в таблице **Orders** можно связать со строкой в таблице **Customers** , но только одну строку в таблице **Customers** можно связать со строкой в таблице **Orders** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-263">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="3b8ca-264">Кроме того, элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-264">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="3b8ca-265">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="3b8ca-265">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="3b8ca-266">Элемент **End** (как дочерний элемент набора **AssociationSet** ) указывает таблицу в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-266">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="3b8ca-267">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-267">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-268">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-268">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-269">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-269">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-270">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-270">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-271">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-271">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="3b8ca-272">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-272">Attribute Name</span></span> | <span data-ttu-id="3b8ca-273">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-273">Is Required</span></span> | <span data-ttu-id="3b8ca-274">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-274">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-275">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-275">**EntitySet**</span></span>  | <span data-ttu-id="3b8ca-276">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-276">Yes</span></span>         | <span data-ttu-id="3b8ca-277">Имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-277">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="3b8ca-278">**Роль**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-278">**Role**</span></span>       | <span data-ttu-id="3b8ca-279">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-279">No</span></span>          | <span data-ttu-id="3b8ca-280">Значение одного из атрибутов **роли** , заданных для одного **конечного** элемента соответствующего элемента Association.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-280">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-281">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-281">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3b8ca-282">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-282">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-283">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-283">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="3b8ca-284">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-284">Example</span></span>

<span data-ttu-id="3b8ca-285">В следующем примере показан элемент **EntityContainer** с элементом **AssociationSet** с двумя элементами **End** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-285">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="3b8ca-286">Элемент EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-286">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-287">Элемент **EntityContainer** в языке SSDL описывает структуру базового источника данных в Entity Framework приложении: наборы сущностей SSDL (определенные в элементах EntitySet) представляют таблицы в базе данных, типы сущностей SSDL (определенные в элементах EntityType) представляют строки в таблице, а наборы ассоциаций (определенные в элементах AssociationSet) представляют ограничения внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-287">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="3b8ca-288">Контейнер сущностей модели хранения сопоставляется с контейнером сущностей концептуальной модели посредством элемента EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-288">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="3b8ca-289">Элемент **EntityContainer** может иметь один или несколько элементов документации.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-289">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="3b8ca-290">Если элемент **Documentation** присутствует, он должен предшествовать всем остальным дочерним элементам.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-290">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="3b8ca-291">Элемент **EntityContainer** может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-291">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-292">EntitySet</span><span class="sxs-lookup"><span data-stu-id="3b8ca-292">EntitySet</span></span>
-   <span data-ttu-id="3b8ca-293">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="3b8ca-293">AssociationSet</span></span>
-   <span data-ttu-id="3b8ca-294">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="3b8ca-294">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-295">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-295">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-296">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-296">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="3b8ca-297">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-297">Attribute Name</span></span> | <span data-ttu-id="3b8ca-298">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-298">Is Required</span></span> | <span data-ttu-id="3b8ca-299">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-299">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-300">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-300">**Name**</span></span>       | <span data-ttu-id="3b8ca-301">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-301">Yes</span></span>         | <span data-ttu-id="3b8ca-302">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-302">The name of the entity container.</span></span> <span data-ttu-id="3b8ca-303">Это имя не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-303">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-304">К элементу **EntityContainer** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-304">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="3b8ca-305">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-305">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-306">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-306">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-307">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-307">Example</span></span>

<span data-ttu-id="3b8ca-308">В следующем примере показан элемент **EntityContainer** , определяющий два набора сущностей и один набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-308">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="3b8ca-309">Обратите внимание, что тип сущности и имена типов ассоциаций определяются именем пространства имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-309">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="3b8ca-310">Элемент EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-310">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-311">Элемент **EntitySet** в языке SSDL представляет таблицу или представление в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-311">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="3b8ca-312">Элемент EntityType в языке SSDL представляет строку в таблице или представлении.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-312">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="3b8ca-313">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-313">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="3b8ca-314">Сопоставление между типом сущности CSDL и типом сущностиSSDL указано в элементе EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-314">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="3b8ca-315">Элемент **EntitySet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-315">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-316">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-316">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-317">DefiningQuery (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-317">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-318">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="3b8ca-318">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-319">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-319">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-320">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-320">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="3b8ca-321">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-321">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="3b8ca-322">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-322">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="3b8ca-323">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-323">Attribute Name</span></span> | <span data-ttu-id="3b8ca-324">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-324">Is Required</span></span> | <span data-ttu-id="3b8ca-325">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-325">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-326">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-326">**Name**</span></span>       | <span data-ttu-id="3b8ca-327">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-327">Yes</span></span>         | <span data-ttu-id="3b8ca-328">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-328">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="3b8ca-329">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-329">**EntityType**</span></span> | <span data-ttu-id="3b8ca-330">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-330">Yes</span></span>         | <span data-ttu-id="3b8ca-331">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-331">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="3b8ca-332">**Схема**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-332">**Schema**</span></span>     | <span data-ttu-id="3b8ca-333">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-333">No</span></span>          | <span data-ttu-id="3b8ca-334">Схема базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-334">The database schema.</span></span>                                                                     |
| <span data-ttu-id="3b8ca-335">**Таблица**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-335">**Table**</span></span>      | <span data-ttu-id="3b8ca-336">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-336">No</span></span>          | <span data-ttu-id="3b8ca-337">Таблица базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-337">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="3b8ca-338">К элементу **EntitySet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-338">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="3b8ca-339">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-339">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-340">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-340">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-341">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-341">Example</span></span>

<span data-ttu-id="3b8ca-342">В следующем примере показан элемент **EntityContainer** , который имеет два элемента **EntitySet** и один элемент **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-342">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="3b8ca-343">Элемент EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-343">EntityType Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-344">Элемент **EntityType** в языке SSDL представляет строку в таблице или представлении базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-344">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="3b8ca-345">Элемент EntitySet в языке SSDL представляет таблицу или представление, в котором появляются строки.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-345">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="3b8ca-346">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-346">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="3b8ca-347">Сопоставление между типом сущности SSDL и типом сущности CSDL указано в элементе EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-347">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="3b8ca-348">Элемент **EntityType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-348">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-349">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-349">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-350">Key (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-350">Key (zero or one element)</span></span>
-   <span data-ttu-id="3b8ca-351">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="3b8ca-351">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-352">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-352">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-353">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-353">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="3b8ca-354">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-354">Attribute Name</span></span> | <span data-ttu-id="3b8ca-355">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-355">Is Required</span></span> | <span data-ttu-id="3b8ca-356">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-356">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-357">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-357">**Name**</span></span>       | <span data-ttu-id="3b8ca-358">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-358">Yes</span></span>         | <span data-ttu-id="3b8ca-359">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-359">The name of the entity type.</span></span> <span data-ttu-id="3b8ca-360">Данное значение обычно совпадает с именем таблицы, в которой тип сущности представляет строку.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-360">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="3b8ca-361">Это значение не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-361">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-362">К элементу **EntityType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-362">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="3b8ca-363">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-363">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-364">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-364">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-365">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-365">Example</span></span>

<span data-ttu-id="3b8ca-366">В следующем примере показан элемент **EntityType** с двумя свойствами:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-366">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="3b8ca-367">Элемент Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-367">Function Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-368">Элемент **Function** в языке SSDL указывает хранимую процедуру, которая существует в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-368">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="3b8ca-369">Элемент **Function** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-369">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-370">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-370">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-371">Параметр (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-371">Parameter (zero or more)</span></span>
-   <span data-ttu-id="3b8ca-372">CommandText (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-372">CommandText (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-373">ReturnType (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-373">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="3b8ca-374">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-374">Annotation elements (zero or more)</span></span>

<span data-ttu-id="3b8ca-375">Тип возвращаемого значения для функции должен быть указан либо как элемент **ReturnType** , либо как атрибут **ReturnType** (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-375">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="3b8ca-376">Хранимая процедура, указанная в модели хранения, может быть импортирована в концептуальную модель приложения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-376">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="3b8ca-377">Дополнительные сведения см. в разделе [запросы с помощью хранимых процедур](xref:ef6/modeling/designer/stored-procedures/query).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-377">For more information, see [Querying with Stored Procedures](xref:ef6/modeling/designer/stored-procedures/query).</span></span> <span data-ttu-id="3b8ca-378">Элемент **Function** также может использоваться для определения пользовательских функций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-378">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-379">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-379">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-380">В следующей таблице описаны атрибуты, которые можно применить к элементу **Function** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-380">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="3b8ca-381">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-381">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="3b8ca-382">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-382">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="3b8ca-383">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-383">Attribute Name</span></span>             | <span data-ttu-id="3b8ca-384">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-384">Is Required</span></span> | <span data-ttu-id="3b8ca-385">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-385">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-386">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-386">**Name**</span></span>                   | <span data-ttu-id="3b8ca-387">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-387">Yes</span></span>         | <span data-ttu-id="3b8ca-388">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-388">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="3b8ca-389">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-389">**ReturnType**</span></span>             | <span data-ttu-id="3b8ca-390">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-390">No</span></span>          | <span data-ttu-id="3b8ca-391">Возвращаемый тип хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-391">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="3b8ca-392">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-392">**Aggregate**</span></span>              | <span data-ttu-id="3b8ca-393">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-393">No</span></span>          | <span data-ttu-id="3b8ca-394">Значение **true** , если хранимая процедура возвращает статистическое значение; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-394">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="3b8ca-395">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-395">**BuiltIn**</span></span>                | <span data-ttu-id="3b8ca-396">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-396">No</span></span>          | <span data-ttu-id="3b8ca-397">**Значение true** , если функция является встроенной функцией<sup>1</sup> ; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-397">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="3b8ca-398">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-398">**StoreFunctionName**</span></span>      | <span data-ttu-id="3b8ca-399">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-399">No</span></span>          | <span data-ttu-id="3b8ca-400">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-400">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="3b8ca-401">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-401">**NiladicFunction**</span></span>        | <span data-ttu-id="3b8ca-402">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-402">No</span></span>          | <span data-ttu-id="3b8ca-403">**Значение true** , если функция является функцией без параметров<sup>2</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-403">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="3b8ca-404">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-404">**IsComposable**</span></span>           | <span data-ttu-id="3b8ca-405">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-405">No</span></span>          | <span data-ttu-id="3b8ca-406">**Значение true** , если функция является составной функцией<sup>3</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-406">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="3b8ca-407">**параметертипесемантикс**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-407">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="3b8ca-408">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-408">No</span></span>          | <span data-ttu-id="3b8ca-409">Перечисление, определяющее семантику типа, которая используется для разрешения перегрузок функций.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-409">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="3b8ca-410">Перечисление определено в манифесте поставщика для определения функции.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-410">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="3b8ca-411">Значение по умолчанию — **алловимплиЦитконверсион**.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-411">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="3b8ca-412">**Схема**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-412">**Schema**</span></span>                 | <span data-ttu-id="3b8ca-413">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-413">No</span></span>          | <span data-ttu-id="3b8ca-414">Имя схемы, в которой определена хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-414">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="3b8ca-415"><sup>1</sup> встроенная функция — это функция, определенная в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-415"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="3b8ca-416">Сведения о функциях, определенных в модели хранения, см. в разделе Элемент CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-416">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="3b8ca-417"><sup>2</sup> функция без параметров не принимает никаких параметров и, при вызове, не требует круглых скобок.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-417"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="3b8ca-418"><sup>3</sup> две функции являются составными, если выходные данные одной функции могут быть входными данными для другой функции.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-418"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="3b8ca-419">К элементу **функции** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-419">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="3b8ca-420">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-420">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-421">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-421">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-422">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-422">Example</span></span>

<span data-ttu-id="3b8ca-423">В следующем примере показан элемент **Function** , соответствующий хранимой процедуре **упдатеордеркуантити** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-423">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="3b8ca-424">Хранимая процедура принимает два параметра и не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-424">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="3b8ca-425">Элемент Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-425">Key Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-426">Элемент **Key** в языке SSDL представляет первичный ключ таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-426">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="3b8ca-427">**Key** — это дочерний элемент элемента EntityType, который представляет строку в таблице.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-427">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="3b8ca-428">Первичный ключ определяется в элементе **Key** путем ссылки на один или несколько элементов свойств, определенных для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-428">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="3b8ca-429">Элемент **Key** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-429">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-430">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-430">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="3b8ca-431">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="3b8ca-431">Annotation elements</span></span>

<span data-ttu-id="3b8ca-432">К элементу **Key** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-432">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-433">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-433">Example</span></span>

<span data-ttu-id="3b8ca-434">В следующем примере показан элемент **EntityType** с ключом, который ссылается на одно свойство:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-434">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="3b8ca-435">Элемент OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-435">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-436">Элемент **OnDelete** в языке SSDL отражает поведение базы данных при удалении строки, участвующей в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-436">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="3b8ca-437">Если для действия задано значение **CASCADE**, то строки, ссылающиеся на удаляемую строку, также будут удалены.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-437">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="3b8ca-438">Если для действия задано значение **нет**, то строки, ссылающиеся на удаляемую строку, также не удаляются.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-438">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="3b8ca-439">Элемент **OnDelete** является дочерним элементом конечного элемента.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-439">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="3b8ca-440">Элемент **OnDelete** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-440">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-441">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-441">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-442">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-442">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-443">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-443">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-444">В следующей таблице описаны атрибуты, которые можно применить к элементу **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-444">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="3b8ca-445">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-445">Attribute Name</span></span> | <span data-ttu-id="3b8ca-446">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-446">Is Required</span></span> | <span data-ttu-id="3b8ca-447">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-447">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-448">**Действие**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-448">**Action**</span></span>     | <span data-ttu-id="3b8ca-449">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-449">Yes</span></span>         | <span data-ttu-id="3b8ca-450">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-450">**Cascade** or **None**.</span></span> <span data-ttu-id="3b8ca-451">( **Ограничение** по значению допустимо, но имеет то же поведение, что и **None**.)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-451">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-452">К элементу **OnDelete** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-452">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="3b8ca-453">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-453">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-454">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-454">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-455">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-455">Example</span></span>

<span data-ttu-id="3b8ca-456">В следующем примере показан элемент **Association** , определяющий ограничение внешнего ключа **FK \_ кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-456">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="3b8ca-457">Элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-457">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="3b8ca-458">Элемент Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-458">Parameter Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-459">Элемент **Parameter** в языке SSDL является дочерним по отношению к элементу Function, который указывает параметры для хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-459">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="3b8ca-460">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-460">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-461">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-461">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-462">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-462">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-463">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-463">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-464">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-464">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="3b8ca-465">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-465">Attribute Name</span></span> | <span data-ttu-id="3b8ca-466">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-466">Is Required</span></span> | <span data-ttu-id="3b8ca-467">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-467">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-468">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-468">**Name**</span></span>       | <span data-ttu-id="3b8ca-469">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-469">Yes</span></span>         | <span data-ttu-id="3b8ca-470">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-470">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3b8ca-471">**Тип**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-471">**Type**</span></span>       | <span data-ttu-id="3b8ca-472">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-472">Yes</span></span>         | <span data-ttu-id="3b8ca-473">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-473">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="3b8ca-474">**Режим**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-474">**Mode**</span></span>       | <span data-ttu-id="3b8ca-475">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-475">No</span></span>          | <span data-ttu-id="3b8ca-476">**В**, **out**или **InOut** в зависимости от того, является ли параметр входным, выходным или входным или выходным.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-476">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="3b8ca-477">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-477">**MaxLength**</span></span>  | <span data-ttu-id="3b8ca-478">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-478">No</span></span>          | <span data-ttu-id="3b8ca-479">Максимальная длина параметра.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-479">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3b8ca-480">**Точность**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-480">**Precision**</span></span>  | <span data-ttu-id="3b8ca-481">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-481">No</span></span>          | <span data-ttu-id="3b8ca-482">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-482">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3b8ca-483">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-483">**Scale**</span></span>      | <span data-ttu-id="3b8ca-484">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-484">No</span></span>          | <span data-ttu-id="3b8ca-485">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-485">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="3b8ca-486">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-486">**SRID**</span></span>       | <span data-ttu-id="3b8ca-487">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-487">No</span></span>          | <span data-ttu-id="3b8ca-488">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-488">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3b8ca-489">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-489">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="3b8ca-490">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-490">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-491">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-491">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="3b8ca-492">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-492">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-493">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-493">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-494">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-494">Example</span></span>

<span data-ttu-id="3b8ca-495">В следующем примере показан элемент **Function** , имеющий два элемента **параметров** , задающих входные параметры:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-495">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="3b8ca-496">Элемент Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-496">Principal Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-497">**Основной** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, который определяет основной элемент ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-497">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="3b8ca-498">Элемент **Principal** указывает первичный ключевой столбец (или столбцы) в таблице, на которую ссылается другой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-498">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="3b8ca-499">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-499">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="3b8ca-500">Зависимый элемент указывает столбцы, которые ссылаются на первичные ключевые столбцы, указанные в элементе **Principal** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-500">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="3b8ca-501">Элемент **Principal** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="3b8ca-501">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3b8ca-502">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-502">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="3b8ca-503">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-503">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-504">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-504">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-505">В следующей таблице описаны атрибуты, которые можно применить к **основному** элементу.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-505">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="3b8ca-506">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-506">Attribute Name</span></span> | <span data-ttu-id="3b8ca-507">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-507">Is Required</span></span> | <span data-ttu-id="3b8ca-508">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-508">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-509">**Роль**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-509">**Role**</span></span>       | <span data-ttu-id="3b8ca-510">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-510">Yes</span></span>         | <span data-ttu-id="3b8ca-511">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей упоминаемый столбец.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-511">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-512">К **основному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-512">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="3b8ca-513">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-513">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-514">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-514">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-515">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-515">Example</span></span>

<span data-ttu-id="3b8ca-516">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK \_ кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-516">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="3b8ca-517">Элемент **Principal** указывает столбец **CustomerID** таблицы **Customer** в качестве основного элемента ограничения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-517">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="3b8ca-518">Элемент Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-518">Property Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-519">Элемент **Property** в языке SSDL представляет столбец в таблице в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-519">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="3b8ca-520">Элементы **свойств** являются дочерними элементами типа EntityType, которые представляют строки в таблице.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-520">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="3b8ca-521">Каждый элемент **Property** , определенный для элемента **EntityType** , представляет столбец.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-521">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="3b8ca-522">Элемент **Свойства** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-522">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-523">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-523">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-524">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-524">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="3b8ca-525">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-525">Attribute Name</span></span>            | <span data-ttu-id="3b8ca-526">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-526">Is Required</span></span> | <span data-ttu-id="3b8ca-527">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-527">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-528">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-528">**Name**</span></span>                  | <span data-ttu-id="3b8ca-529">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-529">Yes</span></span>         | <span data-ttu-id="3b8ca-530">Имя соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-530">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="3b8ca-531">**Тип**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-531">**Type**</span></span>                  | <span data-ttu-id="3b8ca-532">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-532">Yes</span></span>         | <span data-ttu-id="3b8ca-533">Тип соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-533">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="3b8ca-534">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-534">**Nullable**</span></span>              | <span data-ttu-id="3b8ca-535">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-535">No</span></span>          | <span data-ttu-id="3b8ca-536">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли соответствующий столбец иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-536">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="3b8ca-537">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-537">**DefaultValue**</span></span>          | <span data-ttu-id="3b8ca-538">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-538">No</span></span>          | <span data-ttu-id="3b8ca-539">Значение по умолчанию соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-539">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="3b8ca-540">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-540">**MaxLength**</span></span>             | <span data-ttu-id="3b8ca-541">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-541">No</span></span>          | <span data-ttu-id="3b8ca-542">Максимальная длина соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-542">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="3b8ca-543">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-543">**FixedLength**</span></span>           | <span data-ttu-id="3b8ca-544">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-544">No</span></span>          | <span data-ttu-id="3b8ca-545">Значение **true** или **false** в зависимости от того, будет ли значение соответствующего столбца храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-545">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="3b8ca-546">**Точность**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-546">**Precision**</span></span>             | <span data-ttu-id="3b8ca-547">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-547">No</span></span>          | <span data-ttu-id="3b8ca-548">Точность соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-548">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="3b8ca-549">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-549">**Scale**</span></span>                 | <span data-ttu-id="3b8ca-550">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-550">No</span></span>          | <span data-ttu-id="3b8ca-551">Масштаб соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-551">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="3b8ca-552">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-552">**Unicode**</span></span>               | <span data-ttu-id="3b8ca-553">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-553">No</span></span>          | <span data-ttu-id="3b8ca-554">Значение **true** или **false** в зависимости от того, будет ли соответствующее значение столбца храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-554">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="3b8ca-555">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-555">**Collation**</span></span>             | <span data-ttu-id="3b8ca-556">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-556">No</span></span>          | <span data-ttu-id="3b8ca-557">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-557">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="3b8ca-558">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-558">**SRID**</span></span>                  | <span data-ttu-id="3b8ca-559">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-559">No</span></span>          | <span data-ttu-id="3b8ca-560">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-560">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3b8ca-561">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-561">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3b8ca-562">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-562">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3b8ca-563">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-563">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="3b8ca-564">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-564">No</span></span>          | <span data-ttu-id="3b8ca-565">**Нет**, **удостоверение** (если соответствующее значение столбца является удостоверением, созданным в базе данных) или **вычисляемым** (если соответствующее значение столбца вычислено в базе данных).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-565">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="3b8ca-566">Недопустимо для свойств RowType.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-566">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-567">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-567">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="3b8ca-568">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-568">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-569">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-569">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-570">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-570">Example</span></span>

<span data-ttu-id="3b8ca-571">В следующем примере показан элемент **EntityType** с двумя дочерними элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-571">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="3b8ca-572">Элемент PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-572">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-573">Элемент **PropertyRef** в языке SSDL ссылается на свойство, определенное в элементе EntityType, чтобы указать, что свойство будет выполнять одну из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-573">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="3b8ca-574">Быть частью первичного ключа таблицы, которую представляет **Тип EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-574">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="3b8ca-575">Для определения первичного ключа можно использовать один или несколько элементов **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-575">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="3b8ca-576">Дополнительные сведения см. в разделе Элемент Key.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-576">For more information, see Key element.</span></span>
-   <span data-ttu-id="3b8ca-577">Будет зависимым или основным элементом ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-577">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="3b8ca-578">Дополнительные сведения см. в разделе Элемент ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-578">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="3b8ca-579">Элемент **PropertyRef** может иметь только следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-579">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="3b8ca-580">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-580">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-581">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="3b8ca-581">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-582">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-582">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-583">В следующей таблице описаны атрибуты, которые можно применить к элементу **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-583">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="3b8ca-584">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-584">Attribute Name</span></span> | <span data-ttu-id="3b8ca-585">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-585">Is Required</span></span> | <span data-ttu-id="3b8ca-586">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-586">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="3b8ca-587">**имя**;</span><span class="sxs-lookup"><span data-stu-id="3b8ca-587">**Name**</span></span>       | <span data-ttu-id="3b8ca-588">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-588">Yes</span></span>         | <span data-ttu-id="3b8ca-589">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-589">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="3b8ca-590">К элементу **PropertyRef** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-590">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="3b8ca-591">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3b8ca-592">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-593">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-593">Example</span></span>

<span data-ttu-id="3b8ca-594">В следующем примере показан элемент **PropertyRef** , используемый для определения первичного ключа с помощью ссылки на свойство, определенное для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-594">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="3b8ca-595">Элемент ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-595">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-596">Элемент **ReferentialConstraint** в языке SSDL представляет ограничение внешнего ключа (также называемое ограничением ссылочной целостности) в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-596">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="3b8ca-597">Основной и зависимый концы ограничения определяются дочерними элементами Principal и Dependent соответственно.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-597">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="3b8ca-598">Столбцы, которые участвуют в основном и зависимом концах ограничения, указываются элементами PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-598">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="3b8ca-599">Элемент **ReferentialConstraint** является необязательным дочерним элементом элемента Association.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-599">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="3b8ca-600">Если элемент **ReferentialConstraint** не используется для сопоставления ограничения внешнего ключа, указанного в элементе **Association** , для этого необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-600">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="3b8ca-601">Элемент **ReferentialConstraint** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-601">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="3b8ca-602">Documentation (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-602">Documentation (zero or one)</span></span>
-   <span data-ttu-id="3b8ca-603">Principal (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-603">Principal (exactly one)</span></span>
-   <span data-ttu-id="3b8ca-604">Dependent (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-604">Dependent (exactly one)</span></span>
-   <span data-ttu-id="3b8ca-605">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-605">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-606">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-606">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-607">К элементу **ReferentialConstraint** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-607">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="3b8ca-608">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-608">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-609">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-609">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-610">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-610">Example</span></span>

<span data-ttu-id="3b8ca-611">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK \_ кустомерордерс** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-611">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="3b8ca-612">Элемент ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-612">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-613">Элемент **ReturnType** в языке SSDL указывает тип возвращаемого значения для функции, которая определена в элементе **Function** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-613">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="3b8ca-614">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-614">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="3b8ca-615">Тип возвращаемого значения функции указывается с помощью атрибута **Type** или элемента **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-615">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="3b8ca-616">Элемент **ReturnType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-616">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="3b8ca-617">Тип CollectionType (один)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-617">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="3b8ca-618">К элементу **ReturnType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-618">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="3b8ca-619">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-619">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-620">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-620">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-621">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-621">Example</span></span>

<span data-ttu-id="3b8ca-622">В следующем примере используется **функция** , возвращающая коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-622">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="3b8ca-623">Элемент RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-623">RowType Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-624">Элемент **RowType** в языке SSDL определяет неименованную структуру как тип возвращаемого значения для функции, определенной в хранилище.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-624">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="3b8ca-625">Элемент **RowType** является дочерним элементом элемента **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="3b8ca-625">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="3b8ca-626">Элемент **RowType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-626">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="3b8ca-627">Property (один или больше)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-627">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="3b8ca-628">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-628">Example</span></span>

<span data-ttu-id="3b8ca-629">В следующем примере показана функция Store, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-629">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="3b8ca-630">Элемент Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-630">Schema Element (SSDL)</span></span>

<span data-ttu-id="3b8ca-631">Элемент **Schema** в языке SSDL является корневым элементом определения модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-631">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="3b8ca-632">Он содержит определения объектов, функций и контейнеров, из которых состоит модель хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-632">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="3b8ca-633">Элемент **Schema** может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="3b8ca-633">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="3b8ca-634">Взаимосвязь</span><span class="sxs-lookup"><span data-stu-id="3b8ca-634">Association</span></span>
-   <span data-ttu-id="3b8ca-635">EntityType</span><span class="sxs-lookup"><span data-stu-id="3b8ca-635">EntityType</span></span>
-   <span data-ttu-id="3b8ca-636">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="3b8ca-636">EntityContainer</span></span>
-   <span data-ttu-id="3b8ca-637">Функция</span><span class="sxs-lookup"><span data-stu-id="3b8ca-637">Function</span></span>

<span data-ttu-id="3b8ca-638">Элемент **Schema** использует атрибут **Namespace** для определения пространства имен для типа сущности и объектов ассоциаций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-638">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="3b8ca-639">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-639">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="3b8ca-640">Пространство имен модели хранения отличается от пространства имен XML элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-640">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="3b8ca-641">Пространство имен модели хранения (как определено атрибутом **Namespace** ) является логическим контейнером для типов сущностей и типов взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-641">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="3b8ca-642">Пространство имен XML (обозначенное атрибутом **xmlns** ) элемента **Schema** является пространством имен по умолчанию для дочерних элементов и атрибутов элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-642">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="3b8ca-643">Пространства имен XML формы https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (где YYYY и mm представляют год и месяц соответственно) зарезервированы для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-643">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="3b8ca-644">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-644">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3b8ca-645">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="3b8ca-645">Applicable Attributes</span></span>

<span data-ttu-id="3b8ca-646">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **схемы** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-646">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="3b8ca-647">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="3b8ca-647">Attribute Name</span></span>            | <span data-ttu-id="3b8ca-648">Обязательный</span><span class="sxs-lookup"><span data-stu-id="3b8ca-648">Is Required</span></span> | <span data-ttu-id="3b8ca-649">Значение</span><span class="sxs-lookup"><span data-stu-id="3b8ca-649">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-650">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-650">**Namespace**</span></span>             | <span data-ttu-id="3b8ca-651">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-651">Yes</span></span>         | <span data-ttu-id="3b8ca-652">Пространство имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-652">The namespace of the storage model.</span></span> <span data-ttu-id="3b8ca-653">Значение атрибута **Namespace** используется для формирования полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-653">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="3b8ca-654">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, полное имя **EntityType** — пространстве ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-654">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="3b8ca-655">Следующие строки не могут использоваться в качестве значения для атрибута **пространства имен** : **System**, **временные**или **EDM**.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-655">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="3b8ca-656">Значение атрибута **пространства имен** не может совпадать со значением атрибута **Namespace** в элементе CSDL-схемы.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-656">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="3b8ca-657">**Псевдоним**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-657">**Alias**</span></span>                 | <span data-ttu-id="3b8ca-658">Нет</span><span class="sxs-lookup"><span data-stu-id="3b8ca-658">No</span></span>          | <span data-ttu-id="3b8ca-659">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-659">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="3b8ca-660">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, а значение атрибута **Alias** — *Сторажемодел*, то можно использовать сторажемодел. Customer как полное имя **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-660">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="3b8ca-661">**Поставщик**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-661">**Provider**</span></span>              | <span data-ttu-id="3b8ca-662">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-662">Yes</span></span>         | <span data-ttu-id="3b8ca-663">Поставщик данных.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-663">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="3b8ca-664">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-664">**ProviderManifestToken**</span></span> | <span data-ttu-id="3b8ca-665">Да</span><span class="sxs-lookup"><span data-stu-id="3b8ca-665">Yes</span></span>         | <span data-ttu-id="3b8ca-666">Маркер, который указывает поставщику, какой манифест должен быть возвращен.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-666">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="3b8ca-667">Формат маркера не определен.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-667">No format for the token is defined.</span></span> <span data-ttu-id="3b8ca-668">Значения для маркера определяются поставщиком.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-668">Values for the token are defined by the provider.</span></span> <span data-ttu-id="3b8ca-669">Дополнительные сведения о токенах манифеста поставщика SQL Server см. в разделе SqlClient для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-669">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="3b8ca-670">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-670">Example</span></span>

<span data-ttu-id="3b8ca-671">В следующем примере показан элемент **схемы** , содержащий элемент **EntityContainer** , два элемента **EntityType** и один элемент **Association** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-671">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="3b8ca-672">Атрибуты примечаний</span><span class="sxs-lookup"><span data-stu-id="3b8ca-672">Annotation Attributes</span></span>

<span data-ttu-id="3b8ca-673">Атрибуты annotation в языке SSDL представляют собой настраиваемые атрибуты XML в модели хранения, которые содержат дополнительные метаданные об элементах в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-673">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="3b8ca-674">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-674">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="3b8ca-675">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-675">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="3b8ca-676">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-676">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="3b8ca-677">К данному конкретному элементу языка SSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-677">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="3b8ca-678">Метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-678">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-679">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-679">Example</span></span>

<span data-ttu-id="3b8ca-680">В следующем примере показан элемент EntityType с атрибутом аннотации, примененным к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-680">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="3b8ca-681">В примере также показан элемент Annotation, добавленный к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-681">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="3b8ca-682">Элементы Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-682">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="3b8ca-683">Элементы annotation в языке SSDL представляют собой настраиваемые элементы XML в модели хранения, которые содержат дополнительные метаданные о модели хранения.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-683">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="3b8ca-684">Элементы annotation должны не только иметь верную структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-684">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="3b8ca-685">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-685">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="3b8ca-686">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-686">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="3b8ca-687">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-687">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="3b8ca-688">У данного элемента языка SSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-688">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="3b8ca-689">Начиная с .NET Framework версии 4 метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-689">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3b8ca-690">Пример</span><span class="sxs-lookup"><span data-stu-id="3b8ca-690">Example</span></span>

<span data-ttu-id="3b8ca-691">В следующем примере показан элемент EntityType, имеющий элемент Annotation (**кустомелемент**).</span><span class="sxs-lookup"><span data-stu-id="3b8ca-691">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="3b8ca-692">В примере также показан атрибут аннотации, примененный к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-692">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="3b8ca-693">Аспекты (модель SSDL)</span><span class="sxs-lookup"><span data-stu-id="3b8ca-693">Facets (SSDL)</span></span>

<span data-ttu-id="3b8ca-694">Аспекты в языке SSDL представляют ограничения для типов столбцов, указанных в элементах Property.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-694">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="3b8ca-695">Аспекты реализуются как атрибуты XML для элементов **свойств** .</span><span class="sxs-lookup"><span data-stu-id="3b8ca-695">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="3b8ca-696">В следующей таблице описываются аспекты, поддерживаемые в SSDL.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-696">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="3b8ca-697">Аспект</span><span class="sxs-lookup"><span data-stu-id="3b8ca-697">Facet</span></span>           | <span data-ttu-id="3b8ca-698">Описание</span><span class="sxs-lookup"><span data-stu-id="3b8ca-698">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3b8ca-699">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-699">**Collation**</span></span>   | <span data-ttu-id="3b8ca-700">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-700">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="3b8ca-701">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-701">**FixedLength**</span></span> | <span data-ttu-id="3b8ca-702">Указывает, может ли изменяться длина значения столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-702">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="3b8ca-703">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-703">**MaxLength**</span></span>   | <span data-ttu-id="3b8ca-704">Указывает максимальную длину значения столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-704">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="3b8ca-705">**Точность**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-705">**Precision**</span></span>   | <span data-ttu-id="3b8ca-706">Для свойств типа **Decimal**указывает количество цифр, которое может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-706">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="3b8ca-707">Для свойств типа **time**, **DateTime**и **DateTimeOffset**указывается количество цифр для дробной части секунды значения столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-707">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="3b8ca-708">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-708">**Scale**</span></span>       | <span data-ttu-id="3b8ca-709">Задает число цифр справа от десятичной запятой в значении столбца.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-709">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="3b8ca-710">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="3b8ca-710">**Unicode**</span></span>     | <span data-ttu-id="3b8ca-711">Указывает, будет ли значение столбца храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="3b8ca-711">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
