---
title: Спецификация SSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490354"
---
# <a name="ssdl-specification"></a><span data-ttu-id="9bf15-102">Спецификация SSDL</span><span class="sxs-lookup"><span data-stu-id="9bf15-102">SSDL Specification</span></span>
<span data-ttu-id="9bf15-103">SSDL представляет собой язык на основе XML, на котором описывается модель хранения в приложениях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9bf15-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="9bf15-104">В приложении Entity Framework метаданные модели хранения, загружается из SSDL-файла (написанного на языке SSDL) в экземпляр System.Data.Metadata.Edm.StoreItemCollection и доступен с помощью методов в Класс System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="9bf15-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="9bf15-105">Платформа Entity Framework метаданные модели хранения используются для преобразования запросов к концептуальной модели для конкретного хранилища команды.</span><span class="sxs-lookup"><span data-stu-id="9bf15-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="9bf15-106">Entity Framework Designer (конструктор EF) хранит сведения о модели хранения в EDMX-файла во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9bf15-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="9bf15-107">Во время построения конструктор сущностей использует сведения в EDMX-файла, создаваемого SSDL-файл, необходимый Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="9bf15-108">Версии языка SSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="9bf15-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="9bf15-109">Версии языка SSDL</span><span class="sxs-lookup"><span data-stu-id="9bf15-109">SSDL Version</span></span> | <span data-ttu-id="9bf15-110">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="9bf15-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="9bf15-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="9bf15-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="9bf15-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="9bf15-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="9bf15-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="9bf15-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="9bf15-114">Элемент Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-114">Association Element (SSDL)</span></span>

<span data-ttu-id="9bf15-115">**Ассоциации** в языке определения схемы хранения (SSDL) указывает столбцы таблицы, которые участвуют в ограничении внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="9bf15-116">Два элемента обязательный дочерний указать таблицы в концах ассоциации и кратность в каждом элементе.</span><span class="sxs-lookup"><span data-stu-id="9bf15-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="9bf15-117">Необязательный элемент ReferentialConstraint указывает основной и зависимый концы ассоциации, а также участвующих столбцов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="9bf15-118">Если не **ReferentialConstraint** элемент присутствует, элемент AssociationSetMapping должен использоваться для указания сопоставления столбцов для ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9bf15-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="9bf15-119">**Ассоциации** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-120">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-121">End (ровно два)</span><span class="sxs-lookup"><span data-stu-id="9bf15-121">End (exactly two)</span></span>
-   <span data-ttu-id="9bf15-122">ReferentialConstraint (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="9bf15-123">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-124">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-124">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-125">В следующей таблице описаны атрибуты, которые могут применяться к **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="9bf15-126">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-126">Attribute Name</span></span> | <span data-ttu-id="9bf15-127">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-127">Is Required</span></span> | <span data-ttu-id="9bf15-128">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-129">**Name**</span></span>       | <span data-ttu-id="9bf15-130">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-130">Yes</span></span>         | <span data-ttu-id="9bf15-131">Имя соответствующего ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-132">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="9bf15-133">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-134">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-135">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-135">Example</span></span>

<span data-ttu-id="9bf15-136">В следующем примере показан **ассоциации** элемент, использующий **ReferentialConstraint** для указания столбцов, участвующих в элемент **FK\_CustomerOrders**  ограничение внешнего ключа:</span><span class="sxs-lookup"><span data-stu-id="9bf15-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="9bf15-137">Элемент AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="9bf15-138">**AssociationSet** элемент языка определения схемы хранения (SSDL) представляет ограничение внешнего ключа между двумя таблицами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="9bf15-139">Столбцы таблицы, которые участвуют в ограничении внешнего ключа задаются в элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9bf15-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="9bf15-140">**Ассоциации** элемент, соответствующий заданной **AssociationSet** элемент указан в **ассоциации** атрибут **AssociationSet**  элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="9bf15-141">Наборы ассоциаций SSDL сопоставляются наборы ассоциаций CSDL AssociationSetMapping-элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="9bf15-142">Тем не менее, если ассоциация CSDL для сопоставления на основе заданного CSDL определяется с помощью элемента ReferentialConstraint, то соответствующий **AssociationSetMapping** необходим элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="9bf15-143">В этом случае если **AssociationSetMapping** элемент присутствует, то определяемые им сопоставления будут переопределены **ReferentialConstraint** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="9bf15-144">**AssociationSet** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-145">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-146">End (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="9bf15-146">End (zero or two)</span></span>
-   <span data-ttu-id="9bf15-147">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-148">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-148">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-149">В следующей таблице описаны атрибуты, которые могут применяться к **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="9bf15-150">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-150">Attribute Name</span></span>  | <span data-ttu-id="9bf15-151">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-151">Is Required</span></span> | <span data-ttu-id="9bf15-152">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-153">**Name**</span></span>        | <span data-ttu-id="9bf15-154">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-154">Yes</span></span>         | <span data-ttu-id="9bf15-155">Имя ограничения внешнего ключа, представленное набором ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9bf15-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="9bf15-156">**Ассоциации**</span><span class="sxs-lookup"><span data-stu-id="9bf15-156">**Association**</span></span> | <span data-ttu-id="9bf15-157">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-157">Yes</span></span>         | <span data-ttu-id="9bf15-158">Имя ассоциации, определяющее столбцы, которые участвуют в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-159">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="9bf15-160">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-161">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-162">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-162">Example</span></span>

<span data-ttu-id="9bf15-163">В следующем примере показан **AssociationSet** , представляющего `FK_CustomerOrders` ограничения внешнего ключа в основной базе данных:</span><span class="sxs-lookup"><span data-stu-id="9bf15-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="9bf15-164">Элемент CollectionType (язык SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="9bf15-165">**CollectionType** элемент в языке определения схемы хранения (SSDL) указывает, что тип возвращаемого значения функции является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="9bf15-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="9bf15-166">**CollectionType** элемент является дочерним элемента ReturnType.</span><span class="sxs-lookup"><span data-stu-id="9bf15-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="9bf15-167">Тип коллекции задается с помощью дочернего элемента RowType:</span><span class="sxs-lookup"><span data-stu-id="9bf15-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="9bf15-168">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **CollectionType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="9bf15-169">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-170">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-171">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-171">Example</span></span>

<span data-ttu-id="9bf15-172">В следующем примере показано функцию, которая использует **CollectionType** элемента, чтобы указать, что функция возвращает коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="9bf15-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="9bf15-173">Элемент CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="9bf15-174">**CommandText** элемент в языке определения схемы хранения (SSDL) — это дочерний элемент элемента функция, которая позволяет определить инструкцию SQL, выполняемую в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="9bf15-175">**CommandText** элемент позволяет добавить функциональные возможности, схожие с хранимой процедурой в базе данных, но определять **CommandText** элемент в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="9bf15-176">**CommandText** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="9bf15-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="9bf15-177">Тело **CommandText** элемент должен быть допустимой инструкцией SQL в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="9bf15-178">Нет атрибутов, применимых к **CommandText** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-179">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-179">Example</span></span>

<span data-ttu-id="9bf15-180">В следующем примере показан **функция** элемент с дочерним **CommandText** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="9bf15-181">Предоставлять **UpdateProductInOrder** функционировать как метод класса ObjectContext, импортируйте ее в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9bf15-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="9bf15-182">Элемент DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="9bf15-183">**DefiningQuery** элемент в языке определения схемы хранения (SSDL) позволяет выполнять инструкции SQL непосредственно в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="9bf15-184">**DefiningQuery** элемент обычно используется как представление базы данных, но оно определено в модели хранения вместо базы данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="9bf15-185">Представление, определенное в **DefiningQuery** элемент можно сопоставить с типом сущности в концептуальной моделью с применением элемент EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="9bf15-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="9bf15-186">Такие сопоставления предназначены только для чтения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-186">These mappings are read-only.</span></span>  

<span data-ttu-id="9bf15-187">В следующем синтаксисе SSDL показана объявление **EntitySet** следуют **DefiningQuery** элемент, содержащий запрос, используемый для получения представления.</span><span class="sxs-lookup"><span data-stu-id="9bf15-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="9bf15-188">Можно использовать хранимые процедуры в Entity Framework для реализации сценариев чтения и записи применительно к представлениям.</span><span class="sxs-lookup"><span data-stu-id="9bf15-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="9bf15-189">Представление источника данных или представление Entity SQL можно использовать в качестве базовой таблицы для выборки данных и обработки изменений с помощью хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="9bf15-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="9bf15-190">Можно использовать **DefiningQuery** элемента к целевому объекту Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="9bf15-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="9bf15-191">Хотя SQL Server Compact 3.5 не поддерживает хранимые процедуры, вы можете реализовать аналогичные функциональные возможности с **DefiningQuery** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="9bf15-192">Этот элемент также может быть полезен при создании хранимых процедур для преодоления несоответствий между типами данных, используемых в языке программирования, и типами данных источника данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="9bf15-193">Можно написать **DefiningQuery** , принимает определенный набор параметров, а затем вызывает хранимую процедуру с разными наборами параметров, например, хранимую процедуру, которая удаляет данные.</span><span class="sxs-lookup"><span data-stu-id="9bf15-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="9bf15-194">Элемент Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="9bf15-195">**Зависимых** элемент в языке определения схемы хранения (SSDL) является дочерним элементом для элемента ReferentialConstraint, который определяет зависимый конец ограничения внешнего ключа (также называемого справочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="9bf15-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="9bf15-196">**Зависимых** элемент указывает столбец (или столбцы) в таблице, которые ссылаются на первичный ключевой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="9bf15-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="9bf15-197">**PropertyRef** элементы определяют, какие столбцы существуют ссылки.</span><span class="sxs-lookup"><span data-stu-id="9bf15-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="9bf15-198">Основной элемент определяет столбцы первичного ключа, на которые ссылаются столбцы, которые указаны в **зависимых** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="9bf15-199">**Зависимых** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-200">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="9bf15-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9bf15-201">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-202">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-202">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-203">В следующей таблице описаны атрибуты, которые могут применяться к **зависимых** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="9bf15-204">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-204">Attribute Name</span></span> | <span data-ttu-id="9bf15-205">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-205">Is Required</span></span> | <span data-ttu-id="9bf15-206">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-207">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9bf15-207">**Role**</span></span>       | <span data-ttu-id="9bf15-208">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-208">Yes</span></span>         | <span data-ttu-id="9bf15-209">То же значение **роли** атрибута (если используется) соответствующего элемента End; в противном случае имя таблицы, содержащей ссылающийся столбец.</span><span class="sxs-lookup"><span data-stu-id="9bf15-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-210">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **зависимых** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="9bf15-211">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-212">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-213">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-213">Example</span></span>

<span data-ttu-id="9bf15-214">В следующем примере показано элемента ассоциации, использующего **ReferentialConstraint** для указания столбцов, участвующих в элемент **FK\_CustomerOrders** внешнего ключа ограничение.</span><span class="sxs-lookup"><span data-stu-id="9bf15-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9bf15-215">**Зависимых** элемент указывает **CustomerId** столбец **порядок** таблицы как зависимый конец ограничения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="9bf15-216">Элемент Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="9bf15-217">**Документации** можно использовать для предоставления сведений об объекте, который определен в родительском элементе элемента языка определения схемы хранения (SSDL).</span><span class="sxs-lookup"><span data-stu-id="9bf15-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="9bf15-218">**Документации** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-219">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="9bf15-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="9bf15-220">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-220">(zero or one element)</span></span>
-   <span data-ttu-id="9bf15-221">**LongDescription**: подробное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="9bf15-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="9bf15-222">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-223">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-223">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-224">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **документации** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="9bf15-225">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-226">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-227">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-227">Example</span></span>

<span data-ttu-id="9bf15-228">В следующем примере показан **документации** элемент как дочерний элемент элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="9bf15-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="9bf15-229">Элемент End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-229">End Element (SSDL)</span></span>

<span data-ttu-id="9bf15-230">**Окончания** в языке определения схемы хранения (SSDL) определяет таблицу и число строк на одном конце ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="9bf15-231">**Окончания** элемент может быть потомком элемента Association или элемент AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="9bf15-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="9bf15-232">В каждом из этих случаев дочерние элементы и применимые атрибуты будут различными.</span><span class="sxs-lookup"><span data-stu-id="9bf15-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="9bf15-233">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="9bf15-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="9bf15-234">**Окончания** элемент (как дочерний **ассоциации** элемент) задает таблицу и число строк в конце ограничения внешнего ключа в **типа** и **Кратность** атрибутами.</span><span class="sxs-lookup"><span data-stu-id="9bf15-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="9bf15-235">Элементы ограничения внешнего ключа определяются как часть ассоциации SSDL. Ассоциация SSDL должна содержать ровно два элемента.</span><span class="sxs-lookup"><span data-stu-id="9bf15-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="9bf15-236">**Окончания** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-237">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-238">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-239">Элементы annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="9bf15-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-240">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-240">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-241">В следующей таблице описаны атрибуты, которые могут применяться к **окончания** элементу, если он является дочерним элементом **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="9bf15-242">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-242">Attribute Name</span></span>   | <span data-ttu-id="9bf15-243">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-243">Is Required</span></span> | <span data-ttu-id="9bf15-244">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="9bf15-245">**Type**</span></span>         | <span data-ttu-id="9bf15-246">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-246">Yes</span></span>         | <span data-ttu-id="9bf15-247">Полное имя набора сущностей языка SSDL, в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="9bf15-248">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9bf15-248">**Role**</span></span>         | <span data-ttu-id="9bf15-249">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-249">No</span></span>          | <span data-ttu-id="9bf15-250">Значение **роли** атрибут в основном или зависимые от элемента соответствующий элемент ReferentialConstraint (если используется).</span><span class="sxs-lookup"><span data-stu-id="9bf15-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="9bf15-251">**Кратность**</span><span class="sxs-lookup"><span data-stu-id="9bf15-251">**Multiplicity**</span></span> | <span data-ttu-id="9bf15-252">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-252">Yes</span></span>         | <span data-ttu-id="9bf15-253">**1**, **от 0 до 1**, или **\*** в зависимости от количества строк, которые могут быть в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="9bf15-254">**1** указывает на конце ограничения внешнего ключа существует ровно одна строка.</span><span class="sxs-lookup"><span data-stu-id="9bf15-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="9bf15-255">**от 0 до 1** указывает, что существует более одной строки в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="9bf15-256">**\*** Указывает, что на конечной точке ограничения внешнего ключа существует ноль, один или более строк.</span><span class="sxs-lookup"><span data-stu-id="9bf15-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-257">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **окончания** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9bf15-258">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-259">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="9bf15-260">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-260">Example</span></span>

<span data-ttu-id="9bf15-261">В следующем примере показан **ассоциации** элемент, определяющий **FK\_CustomerOrders** ограничение внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9bf15-262">**Кратность** значения, указанные на каждом **окончания** указывают, что многие строки в **заказы** таблицы могут быть связаны со строкой в **клиентов**  таблицы, но только одна строка в **клиентов** таблицы могут быть связаны со строкой в **заказы** таблицы.</span><span class="sxs-lookup"><span data-stu-id="9bf15-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="9bf15-263">Кроме того **OnDelete** элемент указывает, что все строки в **заказы** таблицу, которая ссылается на отдельную строку в **клиентов** таблица будет удалена, если в строке **клиентов** таблица удаляется.</span><span class="sxs-lookup"><span data-stu-id="9bf15-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="9bf15-264">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9bf15-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="9bf15-265">**Окончания** элемент (как дочерний **AssociationSet** элемент) задает таблицу на одном конце ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="9bf15-266">**Окончания** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-267">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-268">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-269">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-269">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-270">В следующей таблице описаны атрибуты, которые могут применяться к **окончания** элементу, если он является дочерним элементом **AssociationSet** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="9bf15-271">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-271">Attribute Name</span></span> | <span data-ttu-id="9bf15-272">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-272">Is Required</span></span> | <span data-ttu-id="9bf15-273">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-274">**Набор EntitySet**</span><span class="sxs-lookup"><span data-stu-id="9bf15-274">**EntitySet**</span></span>  | <span data-ttu-id="9bf15-275">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-275">Yes</span></span>         | <span data-ttu-id="9bf15-276">Имя набора сущностей языка SSDL, находится на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="9bf15-277">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9bf15-277">**Role**</span></span>       | <span data-ttu-id="9bf15-278">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-278">No</span></span>          | <span data-ttu-id="9bf15-279">Значение одного из **роли** атрибутами, заданными на одном **окончания** для соответствующего элемента ассоциации.</span><span class="sxs-lookup"><span data-stu-id="9bf15-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-280">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **окончания** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="9bf15-281">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-282">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="9bf15-283">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-283">Example</span></span>

<span data-ttu-id="9bf15-284">В следующем примере показан **EntityContainer** элемент с **AssociationSet** элемент с двумя **окончания** элементов:</span><span class="sxs-lookup"><span data-stu-id="9bf15-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="9bf15-285">Элемент EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="9bf15-286">**EntityContainer** языка определения схемы хранения (SSDL) описывает структуру базового источника данных в приложении Entity Framework: наборы сущностей SSDL (определенные в элементах EntitySet) представляют таблицы в базы данных, типы сущностей SSDL (определенные в элементах EntityType) представляют строки в таблице, а наборы ассоциаций (определенные в элементах AssociationSet) представляют ограничения внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="9bf15-287">Контейнер сущностей концептуальной модели посредством элемента EntityContainerMapping сопоставляет контейнер сущностей модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="9bf15-288">**EntityContainer** элемент может иметь ноль или один элемент документации.</span><span class="sxs-lookup"><span data-stu-id="9bf15-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="9bf15-289">Если **документации** элемент присутствует, он должен предшествовать всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="9bf15-290">**EntityContainer** элемент может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="9bf15-291">EntitySet</span></span>
-   <span data-ttu-id="9bf15-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="9bf15-292">AssociationSet</span></span>
-   <span data-ttu-id="9bf15-293">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9bf15-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-294">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-294">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-295">В следующей таблице описаны атрибуты, которые могут применяться к **EntityContainer** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="9bf15-296">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-296">Attribute Name</span></span> | <span data-ttu-id="9bf15-297">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-297">Is Required</span></span> | <span data-ttu-id="9bf15-298">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-299">**Name**</span></span>       | <span data-ttu-id="9bf15-300">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-300">Yes</span></span>         | <span data-ttu-id="9bf15-301">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="9bf15-301">The name of the entity container.</span></span> <span data-ttu-id="9bf15-302">Это имя не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="9bf15-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-303">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntityContainer** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="9bf15-304">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-305">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-306">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-306">Example</span></span>

<span data-ttu-id="9bf15-307">В следующем примере показан **EntityContainer** элемент, определяющий два набора сущностей и один набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9bf15-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="9bf15-308">Обратите внимание, что тип сущности и имена типов ассоциаций определяются именем пространства имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9bf15-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="9bf15-309">Элемент EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="9bf15-310">**EntitySet** элемент языка определения схемы хранения (SSDL) представляет таблицу или представление в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="9bf15-311">Элемент EntityType языка SSDL представляет строку в таблице или представлении.</span><span class="sxs-lookup"><span data-stu-id="9bf15-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="9bf15-312">**EntityType** атрибут **EntitySet** элемент указывает определенный тип сущности языка SSDL, представляющий строки в наборе сущностей языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="9bf15-313">Сопоставление набора сущностей языка CSDL и SSDL набор сущностей, указывается в элементе EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="9bf15-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="9bf15-314">**EntitySet** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-315">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-316">DefiningQuery (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-317">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9bf15-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-318">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-318">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-319">В следующей таблице описаны атрибуты, которые могут применяться к **EntitySet** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf15-320">Некоторые атрибуты (не указанные здесь) могут быть дополнены **хранения** псевдоним.</span><span class="sxs-lookup"><span data-stu-id="9bf15-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="9bf15-321">Эти атрибуты используются мастером обновления модели при обновлении модели.</span><span class="sxs-lookup"><span data-stu-id="9bf15-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="9bf15-322">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-322">Attribute Name</span></span> | <span data-ttu-id="9bf15-323">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-323">Is Required</span></span> | <span data-ttu-id="9bf15-324">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-325">**Name**</span></span>       | <span data-ttu-id="9bf15-326">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-326">Yes</span></span>         | <span data-ttu-id="9bf15-327">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9bf15-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="9bf15-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="9bf15-328">**EntityType**</span></span> | <span data-ttu-id="9bf15-329">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-329">Yes</span></span>         | <span data-ttu-id="9bf15-330">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="9bf15-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="9bf15-331">**Схема**</span><span class="sxs-lookup"><span data-stu-id="9bf15-331">**Schema**</span></span>     | <span data-ttu-id="9bf15-332">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-332">No</span></span>          | <span data-ttu-id="9bf15-333">Схема базы данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="9bf15-334">**Таблица**</span><span class="sxs-lookup"><span data-stu-id="9bf15-334">**Table**</span></span>      | <span data-ttu-id="9bf15-335">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-335">No</span></span>          | <span data-ttu-id="9bf15-336">Таблица базы данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="9bf15-337">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntitySet** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="9bf15-338">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-339">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-340">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-340">Example</span></span>

<span data-ttu-id="9bf15-341">В следующем примере показан **EntityContainer** элемент, имеющий два **EntitySet** элементов и один **AssociationSet** элемент:</span><span class="sxs-lookup"><span data-stu-id="9bf15-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="9bf15-342">Элемент EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="9bf15-343">**EntityType** в языке определения схемы хранения (SSDL) представляет строку в таблицу или представление базы данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="9bf15-344">Элемент EntitySet на языке SSDL представляет таблицу или представление, в котором появляются строки.</span><span class="sxs-lookup"><span data-stu-id="9bf15-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="9bf15-345">**EntityType** атрибут **EntitySet** элемент указывает определенный тип сущности языка SSDL, представляющий строки в наборе сущностей языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="9bf15-346">Сопоставление между типом сущности SSDL и типом сущности CSDL указывается в элемент EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="9bf15-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="9bf15-347">**EntityType** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-348">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-349">Ключ (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="9bf15-350">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9bf15-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-351">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-351">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-352">В следующей таблице описаны атрибуты, которые могут применяться к **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="9bf15-353">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-353">Attribute Name</span></span> | <span data-ttu-id="9bf15-354">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-354">Is Required</span></span> | <span data-ttu-id="9bf15-355">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-356">**Name**</span></span>       | <span data-ttu-id="9bf15-357">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-357">Yes</span></span>         | <span data-ttu-id="9bf15-358">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9bf15-358">The name of the entity type.</span></span> <span data-ttu-id="9bf15-359">Данное значение обычно совпадает с именем таблицы, в которой тип сущности представляет строку.</span><span class="sxs-lookup"><span data-stu-id="9bf15-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="9bf15-360">Это значение не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="9bf15-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-361">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="9bf15-362">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-363">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-364">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-364">Example</span></span>

<span data-ttu-id="9bf15-365">В следующем примере показан **EntityType** элемент с двумя свойствами:</span><span class="sxs-lookup"><span data-stu-id="9bf15-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="9bf15-366">Элемент Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-366">Function Element (SSDL)</span></span>

<span data-ttu-id="9bf15-367">**Функция** элемент в языке определения схемы хранения (SSDL) указывает хранимую процедуру, которая существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="9bf15-368">**Функция** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-369">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-370">Параметр (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="9bf15-371">CommandText (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="9bf15-372">ReturnType (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="9bf15-373">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="9bf15-374">Возвращает объект типа для функции должен быть указан с помощью **ReturnType** элемент или **ReturnType** атрибутов (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="9bf15-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="9bf15-375">Хранимая процедура, указанная в модели хранения, может быть импортирована в концептуальную модель приложения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="9bf15-376">Дополнительные сведения см. в разделе [выполнение запросов с помощью хранимых процедур](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="9bf15-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="9bf15-377">**Функция** элемент также может использоваться для определения пользовательских функций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-378">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-378">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-379">В следующей таблице описаны атрибуты, которые могут применяться к **функция** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf15-380">Некоторые атрибуты (не указанные здесь) могут быть дополнены **хранения** псевдоним.</span><span class="sxs-lookup"><span data-stu-id="9bf15-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="9bf15-381">Эти атрибуты используются мастером обновления модели при обновлении модели.</span><span class="sxs-lookup"><span data-stu-id="9bf15-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="9bf15-382">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-382">Attribute Name</span></span>             | <span data-ttu-id="9bf15-383">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-383">Is Required</span></span> | <span data-ttu-id="9bf15-384">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-385">**Name**</span></span>                   | <span data-ttu-id="9bf15-386">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-386">Yes</span></span>         | <span data-ttu-id="9bf15-387">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="9bf15-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9bf15-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="9bf15-388">**ReturnType**</span></span>             | <span data-ttu-id="9bf15-389">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-389">No</span></span>          | <span data-ttu-id="9bf15-390">Возвращаемый тип хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="9bf15-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="9bf15-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="9bf15-391">**Aggregate**</span></span>              | <span data-ttu-id="9bf15-392">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-392">No</span></span>          | <span data-ttu-id="9bf15-393">**Значение true,** Если хранимая процедура возвращает статистическое значение; в противном случае **False**.</span><span class="sxs-lookup"><span data-stu-id="9bf15-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="9bf15-394">**Встроенные**</span><span class="sxs-lookup"><span data-stu-id="9bf15-394">**BuiltIn**</span></span>                | <span data-ttu-id="9bf15-395">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-395">No</span></span>          | <span data-ttu-id="9bf15-396">**Значение true,** Если функция встроена в<sup>1</sup> функции; в противном случае **False**.</span><span class="sxs-lookup"><span data-stu-id="9bf15-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="9bf15-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="9bf15-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="9bf15-398">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-398">No</span></span>          | <span data-ttu-id="9bf15-399">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="9bf15-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9bf15-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="9bf15-400">**NiladicFunction**</span></span>        | <span data-ttu-id="9bf15-401">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-401">No</span></span>          | <span data-ttu-id="9bf15-402">**Значение true,** Если функция является функцией без параметров<sup>2</sup> функции; **False** в противном случае.</span><span class="sxs-lookup"><span data-stu-id="9bf15-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="9bf15-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="9bf15-403">**IsComposable**</span></span>           | <span data-ttu-id="9bf15-404">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-404">No</span></span>          | <span data-ttu-id="9bf15-405">**Значение true,** Если функция составляема<sup>3</sup> функции; **False** в противном случае.</span><span class="sxs-lookup"><span data-stu-id="9bf15-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="9bf15-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="9bf15-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="9bf15-407">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-407">No</span></span>          | <span data-ttu-id="9bf15-408">Перечисление, определяющее семантику типа, которая используется для разрешения перегрузок функций.</span><span class="sxs-lookup"><span data-stu-id="9bf15-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="9bf15-409">Перечисление определено в манифесте поставщика для определения функции.</span><span class="sxs-lookup"><span data-stu-id="9bf15-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="9bf15-410">Значение по умолчанию — **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="9bf15-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="9bf15-411">**Схема**</span><span class="sxs-lookup"><span data-stu-id="9bf15-411">**Schema**</span></span>                 | <span data-ttu-id="9bf15-412">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-412">No</span></span>          | <span data-ttu-id="9bf15-413">Имя схемы, в которой определена хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="9bf15-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="9bf15-414"><sup>1</sup> встроенная функция — это функция, которая определена в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="9bf15-415">Сведения о функциях, которые определены в модели хранения см. в разделе элемент CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="9bf15-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="9bf15-416"><sup>2</sup> функция без параметров — функция, которая не принимает параметры и, при вызове скобки не требуются.</span><span class="sxs-lookup"><span data-stu-id="9bf15-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="9bf15-417"><sup>3</sup> две функции допускают композицию, если выходные данные одной функции может быть входных данных другой функции.</span><span class="sxs-lookup"><span data-stu-id="9bf15-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf15-418">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **функция** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="9bf15-419">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-420">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-421">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-421">Example</span></span>

<span data-ttu-id="9bf15-422">В следующем примере показан **функция** элемент, соответствующий **UpdateOrderQuantity** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="9bf15-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="9bf15-423">Хранимая процедура принимает два параметра и не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="9bf15-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="9bf15-424">Элемент Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-424">Key Element (SSDL)</span></span>

<span data-ttu-id="9bf15-425">**Ключ** элемент языка определения схемы хранения (SSDL) представляет первичный ключ таблицы в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="9bf15-426">**Ключ** является дочерним элементом элемента EntityType, представляющий строку в таблице.</span><span class="sxs-lookup"><span data-stu-id="9bf15-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="9bf15-427">Первичный ключ определен в **ключ** элемент, ссылаясь на один или несколько свойств элементов, которые определены на **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="9bf15-428">**Ключ** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-429">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="9bf15-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9bf15-430">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9bf15-430">Annotation elements</span></span>

<span data-ttu-id="9bf15-431">Нет атрибутов, применимых к **ключ** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-432">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-432">Example</span></span>

<span data-ttu-id="9bf15-433">В следующем примере показан **EntityType** элемент с ключом, который ссылается на одно свойство:</span><span class="sxs-lookup"><span data-stu-id="9bf15-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="9bf15-434">Элемент OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="9bf15-435">**OnDelete** элемент в языке определения схемы хранения (SSDL) отражает поведение базы данных, при удалении строки, которая участвует в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="9bf15-436">Если задано действие **Cascade**, то также будут удалены строки, на которые ссылается на строку, которая удаляется.</span><span class="sxs-lookup"><span data-stu-id="9bf15-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="9bf15-437">Если задано действие **None**, то также не удаляются строки, на которые ссылается на строку, которая удаляется.</span><span class="sxs-lookup"><span data-stu-id="9bf15-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="9bf15-438">**OnDelete** элемент является дочерним элементом элемента End.</span><span class="sxs-lookup"><span data-stu-id="9bf15-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="9bf15-439">**OnDelete** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-440">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-441">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-442">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-442">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-443">В следующей таблице описаны атрибуты, которые могут применяться к **OnDelete** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="9bf15-444">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-444">Attribute Name</span></span> | <span data-ttu-id="9bf15-445">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-445">Is Required</span></span> | <span data-ttu-id="9bf15-446">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-447">**Действие**</span><span class="sxs-lookup"><span data-stu-id="9bf15-447">**Action**</span></span>     | <span data-ttu-id="9bf15-448">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-448">Yes</span></span>         | <span data-ttu-id="9bf15-449">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="9bf15-449">**Cascade** or **None**.</span></span> <span data-ttu-id="9bf15-450">(Значение **Restricted** является допустимым, но действует так же, как **None**.)</span><span class="sxs-lookup"><span data-stu-id="9bf15-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-451">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **OnDelete** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="9bf15-452">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-453">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-454">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-454">Example</span></span>

<span data-ttu-id="9bf15-455">В следующем примере показан **ассоциации** элемент, определяющий **FK\_CustomerOrders** ограничение внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9bf15-456">**OnDelete** элемент указывает, что все строки в **заказы** таблицу, которая ссылается на отдельную строку в **клиентов** таблица будет удалена, если в строке **Клиентов** таблица удаляется.</span><span class="sxs-lookup"><span data-stu-id="9bf15-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="9bf15-457">Элемент Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="9bf15-458">**Параметр** элемент в языке определения схемы хранения (SSDL) является дочерним элемента Function, задающая параметры для хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="9bf15-459">**Параметр** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-460">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-461">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-462">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-462">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-463">В следующей таблице описаны атрибуты, которые могут применяться к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="9bf15-464">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-464">Attribute Name</span></span> | <span data-ttu-id="9bf15-465">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-465">Is Required</span></span> | <span data-ttu-id="9bf15-466">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-467">**Name**</span></span>       | <span data-ttu-id="9bf15-468">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-468">Yes</span></span>         | <span data-ttu-id="9bf15-469">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="9bf15-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="9bf15-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="9bf15-470">**Type**</span></span>       | <span data-ttu-id="9bf15-471">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-471">Yes</span></span>         | <span data-ttu-id="9bf15-472">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="9bf15-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="9bf15-473">**Режим**</span><span class="sxs-lookup"><span data-stu-id="9bf15-473">**Mode**</span></span>       | <span data-ttu-id="9bf15-474">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-474">No</span></span>          | <span data-ttu-id="9bf15-475">**В**, **Out**, или **InOut** в зависимости от того, является ли параметр входной, выходной или параметр ввода вывода.</span><span class="sxs-lookup"><span data-stu-id="9bf15-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="9bf15-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9bf15-476">**MaxLength**</span></span>  | <span data-ttu-id="9bf15-477">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-477">No</span></span>          | <span data-ttu-id="9bf15-478">Максимальная длина параметра.</span><span class="sxs-lookup"><span data-stu-id="9bf15-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="9bf15-479">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9bf15-479">**Precision**</span></span>  | <span data-ttu-id="9bf15-480">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-480">No</span></span>          | <span data-ttu-id="9bf15-481">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="9bf15-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="9bf15-482">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9bf15-482">**Scale**</span></span>      | <span data-ttu-id="9bf15-483">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-483">No</span></span>          | <span data-ttu-id="9bf15-484">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="9bf15-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="9bf15-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9bf15-485">**SRID**</span></span>       | <span data-ttu-id="9bf15-486">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-486">No</span></span>          | <span data-ttu-id="9bf15-487">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="9bf15-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9bf15-488">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="9bf15-489">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9bf15-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-490">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **параметр** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="9bf15-491">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-492">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-493">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-493">Example</span></span>

<span data-ttu-id="9bf15-494">В следующем примере показан **функция** элемент, имеющий два **параметр** элементы, которые определяют входные параметры:</span><span class="sxs-lookup"><span data-stu-id="9bf15-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="9bf15-495">Элемент Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="9bf15-496">**Участника** элемент в языке определения схемы хранения (SSDL) является дочерним элементом для элемента ReferentialConstraint, который определяет основной конец ограничения внешнего ключа (также называемого справочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="9bf15-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="9bf15-497">**Участника** элемент определяет столбец первичного ключа (или столбцы) в таблице, на который ссылается другой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="9bf15-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="9bf15-498">**PropertyRef** элементы определяют, какие столбцы существуют ссылки.</span><span class="sxs-lookup"><span data-stu-id="9bf15-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="9bf15-499">Элемент Dependent указывает столбцы, которые ссылаются на столбцы первичного ключа, которые указаны в **участника** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="9bf15-500">**Участника** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="9bf15-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="9bf15-501">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="9bf15-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="9bf15-502">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-503">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-503">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-504">В следующей таблице описаны атрибуты, которые могут применяться к **участника** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="9bf15-505">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-505">Attribute Name</span></span> | <span data-ttu-id="9bf15-506">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-506">Is Required</span></span> | <span data-ttu-id="9bf15-507">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-508">**Роль**</span><span class="sxs-lookup"><span data-stu-id="9bf15-508">**Role**</span></span>       | <span data-ttu-id="9bf15-509">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-509">Yes</span></span>         | <span data-ttu-id="9bf15-510">То же значение **роли** атрибута (если используется) соответствующего элемента End; в противном случае имя таблицы, содержащей ссылочный столбец.</span><span class="sxs-lookup"><span data-stu-id="9bf15-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-511">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **участника** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="9bf15-512">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-513">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-514">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-514">Example</span></span>

<span data-ttu-id="9bf15-515">В следующем примере показано элемента ассоциации, использующего **ReferentialConstraint** для указания столбцов, участвующих в элемент **FK\_CustomerOrders** внешнего ключа ограничение.</span><span class="sxs-lookup"><span data-stu-id="9bf15-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="9bf15-516">**Участника** элемент указывает **CustomerId** столбец **клиента** таблицу в качестве основной конец ограничения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="9bf15-517">Элемент Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-517">Property Element (SSDL)</span></span>

<span data-ttu-id="9bf15-518">**Свойство** элемент языка определения схемы хранения (SSDL) представляет столбец в таблице в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="9bf15-519">**Свойство** элементы являются дочерними для элементов EntityType, представляющих строки в таблице.</span><span class="sxs-lookup"><span data-stu-id="9bf15-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="9bf15-520">Каждый **свойство** элемент, определенный на **EntityType** элемент представляет столбец.</span><span class="sxs-lookup"><span data-stu-id="9bf15-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="9bf15-521">Объект **свойство** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="9bf15-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-522">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-522">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-523">В следующей таблице описаны атрибуты, которые могут применяться к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="9bf15-524">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-524">Attribute Name</span></span>            | <span data-ttu-id="9bf15-525">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-525">Is Required</span></span> | <span data-ttu-id="9bf15-526">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-527">**Name**</span></span>                  | <span data-ttu-id="9bf15-528">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-528">Yes</span></span>         | <span data-ttu-id="9bf15-529">Имя соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="9bf15-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="9bf15-530">**Type**</span></span>                  | <span data-ttu-id="9bf15-531">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-531">Yes</span></span>         | <span data-ttu-id="9bf15-532">Тип соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="9bf15-533">**Допускающий значение NULL**</span><span class="sxs-lookup"><span data-stu-id="9bf15-533">**Nullable**</span></span>              | <span data-ttu-id="9bf15-534">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-534">No</span></span>          | <span data-ttu-id="9bf15-535">**Значение true,** (значение по умолчанию) или **False** в зависимости от того, может ли соответствующий столбец иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9bf15-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="9bf15-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="9bf15-536">**DefaultValue**</span></span>          | <span data-ttu-id="9bf15-537">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-537">No</span></span>          | <span data-ttu-id="9bf15-538">Значение по умолчанию соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="9bf15-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9bf15-539">**MaxLength**</span></span>             | <span data-ttu-id="9bf15-540">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-540">No</span></span>          | <span data-ttu-id="9bf15-541">Максимальная длина соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="9bf15-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9bf15-542">**FixedLength**</span></span>           | <span data-ttu-id="9bf15-543">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-543">No</span></span>          | <span data-ttu-id="9bf15-544">**Значение true,** или **False** в зависимости от того, будет ли значение соответствующего столбца храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="9bf15-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="9bf15-545">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9bf15-545">**Precision**</span></span>             | <span data-ttu-id="9bf15-546">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-546">No</span></span>          | <span data-ttu-id="9bf15-547">Точность соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="9bf15-548">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9bf15-548">**Scale**</span></span>                 | <span data-ttu-id="9bf15-549">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-549">No</span></span>          | <span data-ttu-id="9bf15-550">Масштаб соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="9bf15-551">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9bf15-551">**Unicode**</span></span>               | <span data-ttu-id="9bf15-552">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-552">No</span></span>          | <span data-ttu-id="9bf15-553">**Значение true,** или **False** в зависимости от того, будет ли значение соответствующего столбца храниться как строка Юникода.</span><span class="sxs-lookup"><span data-stu-id="9bf15-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="9bf15-554">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9bf15-554">**Collation**</span></span>             | <span data-ttu-id="9bf15-555">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-555">No</span></span>          | <span data-ttu-id="9bf15-556">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="9bf15-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="9bf15-557">**SRID**</span></span>                  | <span data-ttu-id="9bf15-558">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-558">No</span></span>          | <span data-ttu-id="9bf15-559">Идентификатор ссылки Пространственные системы.</span><span class="sxs-lookup"><span data-stu-id="9bf15-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="9bf15-560">Допустимо только для свойства пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="9bf15-561">Дополнительные сведения см. в разделе [SRID](http://en.wikipedia.org/wiki/SRID) и [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="9bf15-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="9bf15-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="9bf15-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="9bf15-563">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-563">No</span></span>          | <span data-ttu-id="9bf15-564">**Нет**, **удостоверений** (если соответствующее значение столбца является удостоверением, создается в базе данных), или **вычисленные** (если соответствующее значение столбца вычисляется в базе данных).</span><span class="sxs-lookup"><span data-stu-id="9bf15-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="9bf15-565">Не допустимо для RowType свойства.</span><span class="sxs-lookup"><span data-stu-id="9bf15-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-566">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **свойство** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="9bf15-567">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-568">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-569">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-569">Example</span></span>

<span data-ttu-id="9bf15-570">В следующем примере показан **EntityType** элемент с двумя дочерними **свойство** элементов:</span><span class="sxs-lookup"><span data-stu-id="9bf15-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="9bf15-571">Элемент PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="9bf15-572">**PropertyRef** элемент в языке определения схемы хранения (SSDL) ссылается на свойство, определенное в элементе EntityType, чтобы указать, что свойство будет выступать в одной из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="9bf15-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="9bf15-573">Быть частью первичного ключа таблицы, **EntityType** представляет.</span><span class="sxs-lookup"><span data-stu-id="9bf15-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="9bf15-574">Один или несколько **PropertyRef** элементы можно использовать для определения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="9bf15-575">Дополнительные сведения см. в разделе ключа элемента.</span><span class="sxs-lookup"><span data-stu-id="9bf15-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="9bf15-576">Будет зависимым или основным элементом ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="9bf15-577">Дополнительные сведения см. в разделе элемент ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="9bf15-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="9bf15-578">**PropertyRef** элемент может иметь только следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9bf15-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="9bf15-579">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-580">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="9bf15-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-581">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-581">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-582">В следующей таблице описаны атрибуты, которые могут применяться к **PropertyRef** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="9bf15-583">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-583">Attribute Name</span></span> | <span data-ttu-id="9bf15-584">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-584">Is Required</span></span> | <span data-ttu-id="9bf15-585">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="9bf15-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="9bf15-586">**Name**</span></span>       | <span data-ttu-id="9bf15-587">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-587">Yes</span></span>         | <span data-ttu-id="9bf15-588">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="9bf15-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="9bf15-589">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **PropertyRef** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="9bf15-590">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="9bf15-591">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-592">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-592">Example</span></span>

<span data-ttu-id="9bf15-593">В следующем примере показан **PropertyRef** элемент, используемый для определения первичного ключа путем ссылки на свойства, которое определено на **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="9bf15-594">Элемент ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="9bf15-595">**ReferentialConstraint** элемент языка определения схемы хранения (SSDL) представляет ограничение внешнего ключа (также называемый ограничением ссылочной целостности) в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="9bf15-596">Основной и зависимый концы ограничения определяются участника и зависимые от дочерних элементов, соответственно.</span><span class="sxs-lookup"><span data-stu-id="9bf15-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="9bf15-597">Столбцы, участвующие в основной и зависимый концы находятся ссылки на элементы PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="9bf15-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="9bf15-598">**ReferentialConstraint** элемент — необязательный дочерний элемент элемента Association.</span><span class="sxs-lookup"><span data-stu-id="9bf15-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="9bf15-599">Если **ReferentialConstraint** элемент не используется для сопоставления ограничения внешнего ключа, который указан в **ассоциации** элемент AssociationSetMapping, элемент должен использоваться для этого.</span><span class="sxs-lookup"><span data-stu-id="9bf15-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="9bf15-600">**ReferentialConstraint** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9bf15-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="9bf15-601">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="9bf15-602">Участник (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="9bf15-603">Зависимые (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="9bf15-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="9bf15-604">Элементы annotation (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="9bf15-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-605">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-605">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-606">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ReferentialConstraint** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="9bf15-607">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-608">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-609">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-609">Example</span></span>

<span data-ttu-id="9bf15-610">В следующем примере показан **ассоциации** элемент, использующий **ReferentialConstraint** для указания столбцов, участвующих в элемент **FK\_CustomerOrders**  ограничение внешнего ключа:</span><span class="sxs-lookup"><span data-stu-id="9bf15-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="9bf15-611">Элемент ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="9bf15-612">**ReturnType** элемент в языке определения схемы хранения (SSDL) указывает тип возвращаемого значения для функции, которая определена в **функция** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="9bf15-613">Тип возвращаемого значения также можно задать с помощью функции **ReturnType** атрибута.</span><span class="sxs-lookup"><span data-stu-id="9bf15-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="9bf15-614">Тип возвращаемого значения функции указан с **тип** атрибут или **ReturnType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="9bf15-615">**ReturnType** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9bf15-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="9bf15-616">CollectionType (один)</span><span class="sxs-lookup"><span data-stu-id="9bf15-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="9bf15-617">Любое количество атрибутов annotation (настраиваемых атрибутов XML), которые могут быть применены к **ReturnType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="9bf15-618">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="9bf15-619">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-620">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-620">Example</span></span>

<span data-ttu-id="9bf15-621">В следующем примере используется **функция** , возвращающий коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="9bf15-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="9bf15-622">Элемент RowType (язык SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="9bf15-623">Объект **RowType** элемент в языке определения схемы хранения (SSDL) определяет безымянную структуру как возврат типа для функции, определенной в хранилище.</span><span class="sxs-lookup"><span data-stu-id="9bf15-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="9bf15-624">Объект **RowType** элемент является дочерним элементом элемента **CollectionType** элемент:</span><span class="sxs-lookup"><span data-stu-id="9bf15-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="9bf15-625">Объект **RowType** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="9bf15-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="9bf15-626">Свойство (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="9bf15-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="9bf15-627">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-627">Example</span></span>

<span data-ttu-id="9bf15-628">В следующем примере показано функции хранилища, которая использует **CollectionType** элемента, чтобы указать, что функция возвращает коллекцию строк (как указано в **RowType** элемент).</span><span class="sxs-lookup"><span data-stu-id="9bf15-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="9bf15-629">Элемент Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="9bf15-630">**Схемы** элемент в языке определения схемы хранения (SSDL) является корневым элементом определения модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="9bf15-631">Он содержит определения объектов, функций и контейнеров, из которых состоит модель хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="9bf15-632">**Схемы** элемент может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="9bf15-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="9bf15-633">Ассоциация</span><span class="sxs-lookup"><span data-stu-id="9bf15-633">Association</span></span>
-   <span data-ttu-id="9bf15-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="9bf15-634">EntityType</span></span>
-   <span data-ttu-id="9bf15-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="9bf15-635">EntityContainer</span></span>
-   <span data-ttu-id="9bf15-636">Функция</span><span class="sxs-lookup"><span data-stu-id="9bf15-636">Function</span></span>

<span data-ttu-id="9bf15-637">**Схемы** элемент использует **пространства имен** атрибут определяет пространство имен для объектов сущности типа и ассоциации в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="9bf15-638">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="9bf15-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="9bf15-639">Пространство имен модели хранения отличается от пространства имен XML элемента **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="9bf15-640">Пространство имен модели хранения (как определено **пространства имен** атрибут) — это логический контейнер для типов сущностей и типов ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="9bf15-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="9bf15-641">Пространство имен XML (обозначается **xmlns** атрибут) из **схемы** элемент является пространство имен по умолчанию для дочерних элементов и атрибутов из **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="9bf15-642">Пространства имен XML вида http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (где YYYY и MM представляют год и месяц соответственно) зарезервированы для SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="9bf15-643">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="9bf15-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="9bf15-644">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="9bf15-644">Applicable Attributes</span></span>

<span data-ttu-id="9bf15-645">В следующей таблице описываются атрибуты могут применяться к **схемы** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="9bf15-646">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="9bf15-646">Attribute Name</span></span>            | <span data-ttu-id="9bf15-647">Необходимо</span><span class="sxs-lookup"><span data-stu-id="9bf15-647">Is Required</span></span> | <span data-ttu-id="9bf15-648">Значение</span><span class="sxs-lookup"><span data-stu-id="9bf15-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-649">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="9bf15-649">**Namespace**</span></span>             | <span data-ttu-id="9bf15-650">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-650">Yes</span></span>         | <span data-ttu-id="9bf15-651">Пространство имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-651">The namespace of the storage model.</span></span> <span data-ttu-id="9bf15-652">Значение **пространства имен** атрибут используется, чтобы сформировать полное имя типа.</span><span class="sxs-lookup"><span data-stu-id="9bf15-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="9bf15-653">Например если **EntityType** с именем *клиента* находится в пространстве имен ExampleModel.Store, то полное доменное имя **EntityType** является ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="9bf15-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="9bf15-654">Следующие строки не может использоваться как значение для **пространства имен** атрибут: **системы**, **временных**, или **Edm**.</span><span class="sxs-lookup"><span data-stu-id="9bf15-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="9bf15-655">Значение для **пространства имен** атрибут не может быть таким же, как значение для **пространства имен** атрибут в элементе CSDL-схеме.</span><span class="sxs-lookup"><span data-stu-id="9bf15-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="9bf15-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="9bf15-656">**Alias**</span></span>                 | <span data-ttu-id="9bf15-657">Нет</span><span class="sxs-lookup"><span data-stu-id="9bf15-657">No</span></span>          | <span data-ttu-id="9bf15-658">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9bf15-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="9bf15-659">Например если **EntityType** с именем *клиента* находится в пространстве имен ExampleModel.Store, а значение **псевдоним** атрибут *StorageModel*, то в качестве полное доменное имя можно указать StorageModel.Customer **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="9bf15-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="9bf15-660">**Поставщик**</span><span class="sxs-lookup"><span data-stu-id="9bf15-660">**Provider**</span></span>              | <span data-ttu-id="9bf15-661">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-661">Yes</span></span>         | <span data-ttu-id="9bf15-662">Поставщик данных.</span><span class="sxs-lookup"><span data-stu-id="9bf15-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="9bf15-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="9bf15-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="9bf15-664">Да</span><span class="sxs-lookup"><span data-stu-id="9bf15-664">Yes</span></span>         | <span data-ttu-id="9bf15-665">Маркер, который указывает поставщику, какой манифест должен быть возвращен.</span><span class="sxs-lookup"><span data-stu-id="9bf15-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="9bf15-666">Формат маркера не определен.</span><span class="sxs-lookup"><span data-stu-id="9bf15-666">No format for the token is defined.</span></span> <span data-ttu-id="9bf15-667">Значения для маркера определяются поставщиком.</span><span class="sxs-lookup"><span data-stu-id="9bf15-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="9bf15-668">Сведения о маркерах манифеста поставщика SQL Server см. в разделе SqlClient для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9bf15-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="9bf15-669">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-669">Example</span></span>

<span data-ttu-id="9bf15-670">В следующем примере показан **схемы** элемент, содержащий **EntityContainer** элемент, двух **EntityType** элементов и один **ассоциации** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="9bf15-671">Атрибуты примечаний</span><span class="sxs-lookup"><span data-stu-id="9bf15-671">Annotation Attributes</span></span>

<span data-ttu-id="9bf15-672">Атрибуты annotation в языке SSDL представляют собой настраиваемые атрибуты XML в модели хранения, которые содержат дополнительные метаданные об элементах в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="9bf15-673">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="9bf15-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="9bf15-674">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="9bf15-675">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="9bf15-676">К данному конкретному элементу языка SSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="9bf15-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="9bf15-677">Метаданные, содержащиеся в элементах annotation может осуществляться во время выполнения с помощью классов в пространстве имен System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="9bf15-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-678">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-678">Example</span></span>

<span data-ttu-id="9bf15-679">В следующем примере показано EntityType элемент, имеющий атрибут annotation применяется к **OrderId** свойство.</span><span class="sxs-lookup"><span data-stu-id="9bf15-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="9bf15-680">В примере также показывается элемент заметки, добавляемый **EntityType** элемент.</span><span class="sxs-lookup"><span data-stu-id="9bf15-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="9bf15-681">Элементы Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="9bf15-682">Элементы annotation в языке SSDL представляют собой настраиваемые элементы XML в модели хранения, которые содержат дополнительные метаданные о модели хранения.</span><span class="sxs-lookup"><span data-stu-id="9bf15-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="9bf15-683">Элементы annotation должны не только иметь верную структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="9bf15-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="9bf15-684">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="9bf15-685">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="9bf15-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="9bf15-686">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="9bf15-687">У данного элемента языка SSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="9bf15-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="9bf15-688">Начиная с .NET Framework версии 4, метаданные, содержащиеся в элементах annotation может осуществляться во время выполнения с помощью классов в пространстве имен System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="9bf15-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="9bf15-689">Пример</span><span class="sxs-lookup"><span data-stu-id="9bf15-689">Example</span></span>

<span data-ttu-id="9bf15-690">В следующем примере показано элемент EntityType с элементом annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="9bf15-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="9bf15-691">В примере также показано атрибут annotation применяется к **OrderId** свойство.</span><span class="sxs-lookup"><span data-stu-id="9bf15-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="9bf15-692">Аспекты (модель SSDL)</span><span class="sxs-lookup"><span data-stu-id="9bf15-692">Facets (SSDL)</span></span>

<span data-ttu-id="9bf15-693">Аспекты в языке определения схемы хранения (SSDL) представляют ограничения для типов столбцов, которые указаны в свойств элементов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="9bf15-694">Аспекты реализуются как атрибуты XML на **свойство** элементов.</span><span class="sxs-lookup"><span data-stu-id="9bf15-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="9bf15-695">В следующей таблице описываются аспекты, поддерживаемые в SSDL.</span><span class="sxs-lookup"><span data-stu-id="9bf15-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="9bf15-696">Аспект</span><span class="sxs-lookup"><span data-stu-id="9bf15-696">Facet</span></span>           | <span data-ttu-id="9bf15-697">Описание</span><span class="sxs-lookup"><span data-stu-id="9bf15-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9bf15-698">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="9bf15-698">**Collation**</span></span>   | <span data-ttu-id="9bf15-699">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="9bf15-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="9bf15-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="9bf15-700">**FixedLength**</span></span> | <span data-ttu-id="9bf15-701">Указывает, может ли изменяться длина значения столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="9bf15-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="9bf15-702">**MaxLength**</span></span>   | <span data-ttu-id="9bf15-703">Указывает максимальную длину значения столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="9bf15-704">**Точность**</span><span class="sxs-lookup"><span data-stu-id="9bf15-704">**Precision**</span></span>   | <span data-ttu-id="9bf15-705">Для свойств типа **десятичное**, количество десятичных разрядов, может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="9bf15-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="9bf15-706">Для свойств типа **время**, **DateTime**, и **DateTimeOffset**, указывает количество цифр дробной части секунд значения столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="9bf15-707">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="9bf15-707">**Scale**</span></span>       | <span data-ttu-id="9bf15-708">Задает число цифр справа от десятичной запятой в значении столбца.</span><span class="sxs-lookup"><span data-stu-id="9bf15-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="9bf15-709">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="9bf15-709">**Unicode**</span></span>     | <span data-ttu-id="9bf15-710">Указывает, будет ли значение столбца храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="9bf15-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
