---
title: Спецификация языка SSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182550"
---
# <a name="ssdl-specification"></a><span data-ttu-id="1fc1d-102">Спецификация SSDL</span><span class="sxs-lookup"><span data-stu-id="1fc1d-102">SSDL Specification</span></span>
<span data-ttu-id="1fc1d-103">SSDL представляет собой язык на основе XML, на котором описывается модель хранения в приложениях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="1fc1d-104">В приложении Entity Framework метаданные модели хранения загружаются из SSDL-файла (написанного на языке SSDL) в экземпляр класса System. Data. Metadata. EDM. StoreItemCollection и доступны с помощью методов в Класс System. Data. Metadata. EDM. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="1fc1d-105">Entity Framework использует метаданные модели хранения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="1fc1d-106">Entity Framework Designer (конструктор EF) сохраняет сведения о модели хранения в EDMX-файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="1fc1d-107">Во время сборки Entity Designer использует сведения в EDMX-файле, чтобы создать SSDL-файл, необходимый для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="1fc1d-108">Версии языка SSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="1fc1d-109">Версия на языке SSDL</span><span class="sxs-lookup"><span data-stu-id="1fc1d-109">SSDL Version</span></span> | <span data-ttu-id="1fc1d-110">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="1fc1d-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="1fc1d-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="1fc1d-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="1fc1d-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="1fc1d-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="1fc1d-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="1fc1d-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="1fc1d-114">Элемент Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-114">Association Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-115">Элемент **Association** в языке SSDL указывает столбцы таблицы, которые участвуют в ограничении внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="1fc1d-116">Два обязательных дочерних элемента End указывают таблицы в концах сопоставления и кратность для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="1fc1d-117">Дополнительный элемент ReferentialConstraint указывает основной и зависимый концы сопоставления и участвующие столбцы.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="1fc1d-118">Если элемент **ReferentialConstraint** отсутствует, для указания сопоставлений столбцов для ассоциации необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="1fc1d-119">Элемент **Association** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-120">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-121">Конец (ровно два)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-121">End (exactly two)</span></span>
-   <span data-ttu-id="1fc1d-122">ReferentialConstraint (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-123">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-124">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-124">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-125">В следующей таблице описаны атрибуты, которые можно применить к элементу **Association** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="1fc1d-126">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-126">Attribute Name</span></span> | <span data-ttu-id="1fc1d-127">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-127">Is Required</span></span> | <span data-ttu-id="1fc1d-128">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-129">**Name**</span></span>       | <span data-ttu-id="1fc1d-130">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-130">Yes</span></span>         | <span data-ttu-id="1fc1d-131">Имя соответствующего ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-132">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="1fc1d-133">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-134">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-135">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-135">Example</span></span>

<span data-ttu-id="1fc1d-136">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="1fc1d-137">Элемент AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-138">Элемент **AssociationSet** в языке SSDL представляет ограничение внешнего ключа между двумя таблицами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="1fc1d-139">Столбцы таблицы, принимающие участие в ограничении внешнего ключа, указаны в элементе Association.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="1fc1d-140">Элемент **Association** , соответствующий заданному элементу **AssociationSet** , задается в атрибуте **Association** элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="1fc1d-141">Наборы ассоциаций SSDL сопоставляются с наборами ассоциаций CSDL с помощью элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="1fc1d-142">Однако если сопоставление CSDL для заданного набора ассоциаций CSDL определено с помощью элемента ReferentialConstraint, соответствующий элемент **AssociationSetMapping** не требуется.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="1fc1d-143">В этом случае, если имеется элемент **AssociationSetMapping** , определяемые им сопоставления будут переопределены элементом **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="1fc1d-144">Элемент **AssociationSet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-145">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-146">End (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-146">End (zero or two)</span></span>
-   <span data-ttu-id="1fc1d-147">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-148">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-148">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-149">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="1fc1d-150">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-150">Attribute Name</span></span>  | <span data-ttu-id="1fc1d-151">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-151">Is Required</span></span> | <span data-ttu-id="1fc1d-152">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-153">**Name**</span></span>        | <span data-ttu-id="1fc1d-154">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-154">Yes</span></span>         | <span data-ttu-id="1fc1d-155">Имя ограничения внешнего ключа, представленное набором ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="1fc1d-156">**Связке**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-156">**Association**</span></span> | <span data-ttu-id="1fc1d-157">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-157">Yes</span></span>         | <span data-ttu-id="1fc1d-158">Имя ассоциации, определяющее столбцы, которые участвуют в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-159">К элементу **AssociationSet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="1fc1d-160">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-161">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-162">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-162">Example</span></span>

<span data-ttu-id="1fc1d-163">В следующем примере показан элемент **AssociationSet** , представляющий ограничение внешнего ключа `FK_CustomerOrders` в базовой базе данных:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="1fc1d-164">Элемент CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-165">Элемент **CollectionType** в языке SSDL указывает, что возвращаемый функцией тип является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="1fc1d-166">Элемент **CollectionType** является дочерним по отношению к элементу ReturnType.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="1fc1d-167">Тип коллекции задается с помощью дочернего элемента RowType:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="1fc1d-168">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="1fc1d-169">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-170">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-171">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-171">Example</span></span>

<span data-ttu-id="1fc1d-172">В следующем примере показана функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="1fc1d-173">Элемент CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-174">Элемент **CommandText** в языке SSDL является дочерним элементом элемента Function, который позволяет определить инструкцию SQL, выполняемую в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="1fc1d-175">Элемент **CommandText** позволяет добавлять функции, аналогичные хранимым процедурам в базе данных, но вы определяете элемент **CommandText** в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="1fc1d-176">Элемент **CommandText** не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="1fc1d-177">Тело элемента **CommandText** должно быть допустимой инструкцией SQL для базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="1fc1d-178">К элементу **CommandText** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-179">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-179">Example</span></span>

<span data-ttu-id="1fc1d-180">В следующем примере показан элемент **Function** с дочерним элементом **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="1fc1d-181">Предоставьте функцию **упдатепродуктинордер** как метод в контексте ObjectContext, импортировав его в концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="1fc1d-182">Элемент DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-183">Элемент **DefiningQuery** в языке SSDL позволяет выполнять инструкцию SQL непосредственно в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="1fc1d-184">Элемент **DefiningQuery** обычно используется как представление базы данных, но представление определяется в модели хранения, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="1fc1d-185">Представление, определенное в элементе **DefiningQuery** , может быть сопоставлено с типом сущности в концептуальной модели через элемент EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="1fc1d-186">Такие сопоставления предназначены только для чтения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-186">These mappings are read-only.</span></span>  

<span data-ttu-id="1fc1d-187">В следующем синтаксисе языка SSDL показано объявление **набора EntitySet** , за которым следует элемент **DefiningQuery** , содержащий запрос, используемый для получения представления.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="1fc1d-188">Хранимые процедуры можно использовать в Entity Framework для включения сценариев чтения и записи для представлений.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="1fc1d-189"> В качестве базовой таблицы для получения данных и обработки изменений хранимыми процедурами можно использовать либо представление источника данных, либо представление Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="1fc1d-190">Элемент **DefiningQuery** можно использовать для назначения Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="1fc1d-191">Хотя SQL Server Compact 3,5 не поддерживает хранимые процедуры, можно реализовать аналогичные функции с помощью элемента **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="1fc1d-192">Этот элемент также может быть полезен при создании хранимых процедур для преодоления несоответствий между типами данных, используемых в языке программирования, и типами данных источника данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="1fc1d-193">Можно написать параметр **DefiningQuery** , который принимает определенный набор параметров, а затем вызывает хранимую процедуру с другим набором параметров, например хранимую процедуру, которая удаляет данные.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="1fc1d-194">Элемент Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-195">**Зависимый** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, определяющего зависимый конец ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="1fc1d-196">**Зависимый** элемент указывает столбец (или столбцы) в таблице, ссылающейся на первичный ключевой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="1fc1d-197">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="1fc1d-198">Элемент Principal задает столбцы первичного ключа, на которые ссылаются столбцы, указанные в **зависимом** элементе.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="1fc1d-199">**Зависимый** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-200">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="1fc1d-201">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-202">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-202">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-203">В следующей таблице описаны атрибуты, которые можно применить к **зависимому** элементу.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="1fc1d-204">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-204">Attribute Name</span></span> | <span data-ttu-id="1fc1d-205">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-205">Is Required</span></span> | <span data-ttu-id="1fc1d-206">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-207">**Роль**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-207">**Role**</span></span>       | <span data-ttu-id="1fc1d-208">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-208">Yes</span></span>         | <span data-ttu-id="1fc1d-209">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей ссылающийся столбец.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-210">К **зависимому** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="1fc1d-211">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-212">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-213">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-213">Example</span></span>

<span data-ttu-id="1fc1d-214">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="1fc1d-215">**Зависимый** элемент указывает столбец **CustomerID** таблицы **Order** как зависимый конец ограничения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="1fc1d-216">Элемент Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-217">Элемент **Documentation** в языке SSDL может использоваться для предоставления сведений об объекте, определенном в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="1fc1d-218">Элемент **Documentation** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-219">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="1fc1d-220">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-220">(zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-221">**Лонгдескриптион**: расширенное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="1fc1d-222">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-223">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-223">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-224">К элементу **Documentation** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="1fc1d-225">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-226">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-227">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-227">Example</span></span>

<span data-ttu-id="1fc1d-228">В следующем примере показан элемент **Documentation** в качестве дочернего элемента элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="1fc1d-229">Элемент End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-229">End Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-230">Элемент **End** в языке SSDL указывает таблицу и количество строк в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="1fc1d-231">Элемент **End** может быть дочерним по отношению к элементу Association или элементу AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="1fc1d-232">В каждом из этих случаев дочерние элементы и применимые атрибуты будут различными.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="1fc1d-233">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="1fc1d-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="1fc1d-234">Элемент **End** (в качестве дочернего элемента **ассоциации** ) указывает таблицу и число строк в конце ограничения внешнего ключа с атрибутами **тип** и **кратность** соответственно.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="1fc1d-235">Элементы ограничения внешнего ключа определяются как часть ассоциации SSDL. Ассоциация SSDL должна содержать ровно два элемента.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="1fc1d-236">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-237">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-238">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-239">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-240">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-240">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-241">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **Association** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="1fc1d-242">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-242">Attribute Name</span></span>   | <span data-ttu-id="1fc1d-243">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-243">Is Required</span></span> | <span data-ttu-id="1fc1d-244">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-245">**Тип**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-245">**Type**</span></span>         | <span data-ttu-id="1fc1d-246">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-246">Yes</span></span>         | <span data-ttu-id="1fc1d-247">Полное имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="1fc1d-248">**Роль**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-248">**Role**</span></span>         | <span data-ttu-id="1fc1d-249">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-249">No</span></span>          | <span data-ttu-id="1fc1d-250">Значение атрибута **Role** либо в основном, либо в зависимом элементе соответствующего элемента ReferentialConstraint (если используется).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="1fc1d-251">**Количества**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-251">**Multiplicity**</span></span> | <span data-ttu-id="1fc1d-252">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-252">Yes</span></span>         | <span data-ttu-id="1fc1d-253">**1**, **0.1**или **\*** в зависимости от числа строк, которые могут находиться в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="1fc1d-254">значение **1** указывает, что в конце ограничения внешнего ключа существует ровно одна строка.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="1fc1d-255">**0.. 1** указывает, что в окончании ограничения внешнего ключа находится ноль или одна строка.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="1fc1d-256">**\*** указывает, что в окончании ограничения внешнего ключа существует ноль, одна или несколько строк.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-257">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="1fc1d-258">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-259">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="1fc1d-260">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-260">Example</span></span>

<span data-ttu-id="1fc1d-261">В следующем примере показан элемент **Association** , который определяет ограничение внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="1fc1d-262">Значения **кратности** , заданные для каждого **конечного** элемента, указывают на то, что многие строки в таблице **Orders** можно связать со строкой в таблице **Customers** , но только одну строку в таблице **Customers** можно связать со строкой в таблице **Orders** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="1fc1d-263">Кроме того, элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="1fc1d-264">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="1fc1d-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="1fc1d-265">Элемент **End** (как дочерний элемент набора **AssociationSet** ) указывает таблицу в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="1fc1d-266">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-267">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-268">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-269">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-269">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-270">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="1fc1d-271">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-271">Attribute Name</span></span> | <span data-ttu-id="1fc1d-272">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-272">Is Required</span></span> | <span data-ttu-id="1fc1d-273">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-274">**EntitySet**</span></span>  | <span data-ttu-id="1fc1d-275">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-275">Yes</span></span>         | <span data-ttu-id="1fc1d-276">Имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="1fc1d-277">**Роль**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-277">**Role**</span></span>       | <span data-ttu-id="1fc1d-278">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-278">No</span></span>          | <span data-ttu-id="1fc1d-279">Значение одного из атрибутов **роли** , заданных для одного **конечного** элемента соответствующего элемента Association.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-280">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="1fc1d-281">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-282">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="1fc1d-283">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-283">Example</span></span>

<span data-ttu-id="1fc1d-284">В следующем примере показан элемент **EntityContainer** с элементом **AssociationSet** с двумя элементами **End** :</span><span class="sxs-lookup"><span data-stu-id="1fc1d-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="1fc1d-285">Элемент EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-286">Элемент **EntityContainer** в языке SSDL описывает структуру базового источника данных в Entity Framework приложении: наборы сущностей SSDL (определенные в элементах EntitySet) представляют таблицы в базе данных, типы сущностей SSDL (определенные в элементах EntityType) представляют строки в таблице, а наборы ассоциаций (определенные в элементах AssociationSet) представляют ограничения внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="1fc1d-287">Контейнер сущностей модели хранения сопоставляется с контейнером сущностей концептуальной модели посредством элемента EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="1fc1d-288">Элемент **EntityContainer** может иметь один или несколько элементов документации.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="1fc1d-289">Если элемент **Documentation** присутствует, он должен предшествовать всем остальным дочерним элементам.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="1fc1d-290">Элемент **EntityContainer** может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="1fc1d-291">EntitySet</span></span>
-   <span data-ttu-id="1fc1d-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="1fc1d-292">AssociationSet</span></span>
-   <span data-ttu-id="1fc1d-293">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="1fc1d-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-294">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-294">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-295">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="1fc1d-296">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-296">Attribute Name</span></span> | <span data-ttu-id="1fc1d-297">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-297">Is Required</span></span> | <span data-ttu-id="1fc1d-298">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-299">**Name**</span></span>       | <span data-ttu-id="1fc1d-300">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-300">Yes</span></span>         | <span data-ttu-id="1fc1d-301">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-301">The name of the entity container.</span></span> <span data-ttu-id="1fc1d-302">Это имя не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-303">К элементу **EntityContainer** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="1fc1d-304">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-305">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-306">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-306">Example</span></span>

<span data-ttu-id="1fc1d-307">В следующем примере показан элемент **EntityContainer** , определяющий два набора сущностей и один набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="1fc1d-308">Обратите внимание, что тип сущности и имена типов ассоциаций определяются именем пространства имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="1fc1d-309">Элемент EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-310">Элемент **EntitySet** в языке SSDL представляет таблицу или представление в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="1fc1d-311">Элемент EntityType в языке SSDL представляет строку в таблице или представлении.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="1fc1d-312">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="1fc1d-313">Сопоставление между типом сущности CSDL и типом сущностиSSDL указано в элементе EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="1fc1d-314">Элемент **EntitySet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-315">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-316">DefiningQuery (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-317">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="1fc1d-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-318">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-318">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-319">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="1fc1d-320">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="1fc1d-321">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="1fc1d-322">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-322">Attribute Name</span></span> | <span data-ttu-id="1fc1d-323">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-323">Is Required</span></span> | <span data-ttu-id="1fc1d-324">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-325">**Name**</span></span>       | <span data-ttu-id="1fc1d-326">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-326">Yes</span></span>         | <span data-ttu-id="1fc1d-327">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="1fc1d-328">**Типе**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-328">**EntityType**</span></span> | <span data-ttu-id="1fc1d-329">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-329">Yes</span></span>         | <span data-ttu-id="1fc1d-330">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="1fc1d-331">**Схема**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-331">**Schema**</span></span>     | <span data-ttu-id="1fc1d-332">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-332">No</span></span>          | <span data-ttu-id="1fc1d-333">Схема базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="1fc1d-334">**Таблица**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-334">**Table**</span></span>      | <span data-ttu-id="1fc1d-335">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-335">No</span></span>          | <span data-ttu-id="1fc1d-336">Таблица базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="1fc1d-337">К элементу **EntitySet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="1fc1d-338">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-339">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-340">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-340">Example</span></span>

<span data-ttu-id="1fc1d-341">В следующем примере показан элемент **EntityContainer** , который имеет два элемента **EntitySet** и один элемент **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="1fc1d-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="1fc1d-342">Элемент EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-343">Элемент **EntityType** в языке SSDL представляет строку в таблице или представлении базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="1fc1d-344">Элемент EntitySet в языке SSDL представляет таблицу или представление, в котором появляются строки.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="1fc1d-345">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="1fc1d-346">Сопоставление между типом сущности SSDL и типом сущности CSDL указано в элементе EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="1fc1d-347">Элемент **EntityType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-348">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-349">Key (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="1fc1d-350">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="1fc1d-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-351">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-351">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-352">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="1fc1d-353">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-353">Attribute Name</span></span> | <span data-ttu-id="1fc1d-354">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-354">Is Required</span></span> | <span data-ttu-id="1fc1d-355">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-356">**Name**</span></span>       | <span data-ttu-id="1fc1d-357">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-357">Yes</span></span>         | <span data-ttu-id="1fc1d-358">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-358">The name of the entity type.</span></span> <span data-ttu-id="1fc1d-359">Данное значение обычно совпадает с именем таблицы, в которой тип сущности представляет строку.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="1fc1d-360">Это значение не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-361">К элементу **EntityType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="1fc1d-362">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-363">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-364">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-364">Example</span></span>

<span data-ttu-id="1fc1d-365">В следующем примере показан элемент **EntityType** с двумя свойствами:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="1fc1d-366">Элемент Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-366">Function Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-367">Элемент **Function** в языке SSDL указывает хранимую процедуру, которая существует в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="1fc1d-368">Элемент **Function** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-369">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-370">Параметр (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="1fc1d-371">CommandText (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-372">ReturnType (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="1fc1d-373">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="1fc1d-374">Тип возвращаемого значения для функции должен быть указан либо как элемент **ReturnType** , либо как атрибут **ReturnType** (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="1fc1d-375">Хранимая процедура, указанная в модели хранения, может быть импортирована в концептуальную модель приложения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="1fc1d-376">Дополнительные сведения см. в разделе [запросы с помощью хранимых процедур](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="1fc1d-377">Элемент **Function** также может использоваться для определения пользовательских функций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-378">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-378">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-379">В следующей таблице описаны атрибуты, которые можно применить к элементу **Function** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="1fc1d-380">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="1fc1d-381">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="1fc1d-382">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-382">Attribute Name</span></span>             | <span data-ttu-id="1fc1d-383">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-383">Is Required</span></span> | <span data-ttu-id="1fc1d-384">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-385">**Name**</span></span>                   | <span data-ttu-id="1fc1d-386">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-386">Yes</span></span>         | <span data-ttu-id="1fc1d-387">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="1fc1d-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-388">**ReturnType**</span></span>             | <span data-ttu-id="1fc1d-389">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-389">No</span></span>          | <span data-ttu-id="1fc1d-390">Возвращаемый тип хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="1fc1d-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-391">**Aggregate**</span></span>              | <span data-ttu-id="1fc1d-392">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-392">No</span></span>          | <span data-ttu-id="1fc1d-393">Значение **true** , если хранимая процедура возвращает статистическое значение; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="1fc1d-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-394">**BuiltIn**</span></span>                | <span data-ttu-id="1fc1d-395">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-395">No</span></span>          | <span data-ttu-id="1fc1d-396">**Значение true** , если функция является встроенной функцией<sup>1</sup> ; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="1fc1d-397">**сторефунктионнаме**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="1fc1d-398">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-398">No</span></span>          | <span data-ttu-id="1fc1d-399">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="1fc1d-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-400">**NiladicFunction**</span></span>        | <span data-ttu-id="1fc1d-401">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-401">No</span></span>          | <span data-ttu-id="1fc1d-402">**Значение true** , если функция является функцией без параметров<sup>2</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="1fc1d-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-403">**IsComposable**</span></span>           | <span data-ttu-id="1fc1d-404">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-404">No</span></span>          | <span data-ttu-id="1fc1d-405">**Значение true** , если функция является составной функцией<sup>3</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="1fc1d-406">**параметертипесемантикс**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="1fc1d-407">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-407">No</span></span>          | <span data-ttu-id="1fc1d-408">Перечисление, определяющее семантику типа, которая используется для разрешения перегрузок функций.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="1fc1d-409">Перечисление определено в манифесте поставщика для определения функции.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="1fc1d-410">Значение по умолчанию — **алловимплиЦитконверсион**.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="1fc1d-411">**Схема**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-411">**Schema**</span></span>                 | <span data-ttu-id="1fc1d-412">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-412">No</span></span>          | <span data-ttu-id="1fc1d-413">Имя схемы, в которой определена хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="1fc1d-414"><sup>1</sup> встроенная функция — это функция, определенная в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="1fc1d-415">Сведения о функциях, определенных в модели хранения, см. в разделе Элемент CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="1fc1d-416"><sup>2</sup> функция без параметров не принимает никаких параметров и, при вызове, не требует круглых скобок.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="1fc1d-417"><sup>3</sup> две функции являются составными, если выходные данные одной функции могут быть входными данными для другой функции.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="1fc1d-418">К элементу **функции** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="1fc1d-419">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-420">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-421">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-421">Example</span></span>

<span data-ttu-id="1fc1d-422">В следующем примере показан элемент **Function** , соответствующий хранимой процедуре **упдатеордеркуантити** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="1fc1d-423">Хранимая процедура принимает два параметра и не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="1fc1d-424">Элемент Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-424">Key Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-425">Элемент **Key** в языке SSDL представляет первичный ключ таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="1fc1d-426">**Key** — это дочерний элемент элемента EntityType, который представляет строку в таблице.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="1fc1d-427">Первичный ключ определяется в элементе **Key** путем ссылки на один или несколько элементов свойств, определенных для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="1fc1d-428">Элемент **Key** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-429">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="1fc1d-430">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="1fc1d-430">Annotation elements</span></span>

<span data-ttu-id="1fc1d-431">К элементу **Key** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-432">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-432">Example</span></span>

<span data-ttu-id="1fc1d-433">В следующем примере показан элемент **EntityType** с ключом, который ссылается на одно свойство:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="1fc1d-434">Элемент OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-435">Элемент **OnDelete** в языке SSDL отражает поведение базы данных при удалении строки, участвующей в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="1fc1d-436">Если для действия задано значение **CASCADE**, то строки, ссылающиеся на удаляемую строку, также будут удалены.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="1fc1d-437">Если для действия задано значение **нет**, то строки, ссылающиеся на удаляемую строку, также не удаляются.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="1fc1d-438">Элемент **OnDelete** является дочерним элементом конечного элемента.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="1fc1d-439">Элемент **OnDelete** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-440">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-441">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-442">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-442">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-443">В следующей таблице описаны атрибуты, которые можно применить к элементу **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="1fc1d-444">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-444">Attribute Name</span></span> | <span data-ttu-id="1fc1d-445">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-445">Is Required</span></span> | <span data-ttu-id="1fc1d-446">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-447">**Действие**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-447">**Action**</span></span>     | <span data-ttu-id="1fc1d-448">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-448">Yes</span></span>         | <span data-ttu-id="1fc1d-449">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-449">**Cascade** or **None**.</span></span> <span data-ttu-id="1fc1d-450">( **Ограничение** по значению допустимо, но имеет то же поведение, что и **None**.)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-451">К элементу **OnDelete** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="1fc1d-452">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-453">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-454">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-454">Example</span></span>

<span data-ttu-id="1fc1d-455">В следующем примере показан элемент **Association** , который определяет ограничение внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="1fc1d-456">Элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="1fc1d-457">Элемент Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-458">Элемент **Parameter** в языке SSDL является дочерним по отношению к элементу Function, который указывает параметры для хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="1fc1d-459">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-460">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-461">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-462">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-462">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-463">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="1fc1d-464">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-464">Attribute Name</span></span> | <span data-ttu-id="1fc1d-465">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-465">Is Required</span></span> | <span data-ttu-id="1fc1d-466">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-467">**Name**</span></span>       | <span data-ttu-id="1fc1d-468">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-468">Yes</span></span>         | <span data-ttu-id="1fc1d-469">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="1fc1d-470">**Тип**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-470">**Type**</span></span>       | <span data-ttu-id="1fc1d-471">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-471">Yes</span></span>         | <span data-ttu-id="1fc1d-472">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="1fc1d-473">**Режим**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-473">**Mode**</span></span>       | <span data-ttu-id="1fc1d-474">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-474">No</span></span>          | <span data-ttu-id="1fc1d-475">**В**, **out**или **InOut** в зависимости от того, является ли параметр входным, выходным или входным или выходным.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="1fc1d-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-476">**MaxLength**</span></span>  | <span data-ttu-id="1fc1d-477">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-477">No</span></span>          | <span data-ttu-id="1fc1d-478">Максимальная длина параметра.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="1fc1d-479">**Точность**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-479">**Precision**</span></span>  | <span data-ttu-id="1fc1d-480">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-480">No</span></span>          | <span data-ttu-id="1fc1d-481">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="1fc1d-482">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-482">**Scale**</span></span>      | <span data-ttu-id="1fc1d-483">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-483">No</span></span>          | <span data-ttu-id="1fc1d-484">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="1fc1d-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-485">**SRID**</span></span>       | <span data-ttu-id="1fc1d-486">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-486">No</span></span>          | <span data-ttu-id="1fc1d-487">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="1fc1d-488">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="1fc1d-489">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-490">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="1fc1d-491">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-492">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-493">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-493">Example</span></span>

<span data-ttu-id="1fc1d-494">В следующем примере показан элемент **Function** , имеющий два элемента **параметров** , задающих входные параметры:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="1fc1d-495">Элемент Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-496">**Основной** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, который определяет основной элемент ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="1fc1d-497">Элемент **Principal** указывает первичный ключевой столбец (или столбцы) в таблице, на которую ссылается другой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="1fc1d-498">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="1fc1d-499">Зависимый элемент указывает столбцы, которые ссылаются на первичные ключевые столбцы, указанные в элементе **Principal** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="1fc1d-500">Элемент **Principal** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="1fc1d-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="1fc1d-501">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="1fc1d-502">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-503">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-503">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-504">В следующей таблице описаны атрибуты, которые можно применить к **основному** элементу.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="1fc1d-505">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-505">Attribute Name</span></span> | <span data-ttu-id="1fc1d-506">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-506">Is Required</span></span> | <span data-ttu-id="1fc1d-507">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-508">**Роль**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-508">**Role**</span></span>       | <span data-ttu-id="1fc1d-509">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-509">Yes</span></span>         | <span data-ttu-id="1fc1d-510">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей упоминаемый столбец.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-511">К **основному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="1fc1d-512">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-513">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-514">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-514">Example</span></span>

<span data-ttu-id="1fc1d-515">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="1fc1d-516">Элемент **Principal** указывает столбец **CustomerID** таблицы **Customer** в качестве основного элемента ограничения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="1fc1d-517">Элемент Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-517">Property Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-518">Элемент **Property** в языке SSDL представляет столбец в таблице в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="1fc1d-519">Элементы **свойств** являются дочерними элементами типа EntityType, которые представляют строки в таблице.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="1fc1d-520">Каждый элемент **Property** , определенный для элемента **EntityType** , представляет столбец.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="1fc1d-521">Элемент **Свойства** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-522">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-522">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-523">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="1fc1d-524">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-524">Attribute Name</span></span>            | <span data-ttu-id="1fc1d-525">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-525">Is Required</span></span> | <span data-ttu-id="1fc1d-526">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-527">**Name**</span></span>                  | <span data-ttu-id="1fc1d-528">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-528">Yes</span></span>         | <span data-ttu-id="1fc1d-529">Имя соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="1fc1d-530">**Тип**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-530">**Type**</span></span>                  | <span data-ttu-id="1fc1d-531">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-531">Yes</span></span>         | <span data-ttu-id="1fc1d-532">Тип соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="1fc1d-533">**Обнуляемого**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-533">**Nullable**</span></span>              | <span data-ttu-id="1fc1d-534">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-534">No</span></span>          | <span data-ttu-id="1fc1d-535">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли соответствующий столбец иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="1fc1d-536">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-536">**DefaultValue**</span></span>          | <span data-ttu-id="1fc1d-537">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-537">No</span></span>          | <span data-ttu-id="1fc1d-538">Значение по умолчанию соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="1fc1d-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-539">**MaxLength**</span></span>             | <span data-ttu-id="1fc1d-540">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-540">No</span></span>          | <span data-ttu-id="1fc1d-541">Максимальная длина соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="1fc1d-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-542">**FixedLength**</span></span>           | <span data-ttu-id="1fc1d-543">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-543">No</span></span>          | <span data-ttu-id="1fc1d-544">Значение **true** или **false** в зависимости от того, будет ли значение соответствующего столбца храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="1fc1d-545">**Точность**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-545">**Precision**</span></span>             | <span data-ttu-id="1fc1d-546">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-546">No</span></span>          | <span data-ttu-id="1fc1d-547">Точность соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="1fc1d-548">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-548">**Scale**</span></span>                 | <span data-ttu-id="1fc1d-549">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-549">No</span></span>          | <span data-ttu-id="1fc1d-550">Масштаб соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="1fc1d-551">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-551">**Unicode**</span></span>               | <span data-ttu-id="1fc1d-552">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-552">No</span></span>          | <span data-ttu-id="1fc1d-553">Значение **true** или **false** в зависимости от того, будет ли соответствующее значение столбца храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="1fc1d-554">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-554">**Collation**</span></span>             | <span data-ttu-id="1fc1d-555">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-555">No</span></span>          | <span data-ttu-id="1fc1d-556">Строка, указывающая последовательность сортировки, которая должна использоваться в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="1fc1d-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-557">**SRID**</span></span>                  | <span data-ttu-id="1fc1d-558">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-558">No</span></span>          | <span data-ttu-id="1fc1d-559">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="1fc1d-560">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="1fc1d-561">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="1fc1d-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="1fc1d-563">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-563">No</span></span>          | <span data-ttu-id="1fc1d-564">**Нет**, **удостоверение** (если соответствующее значение столбца является удостоверением, созданным в базе данных) или **вычисляемым** (если соответствующее значение столбца вычислено в базе данных).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="1fc1d-565">Недопустимо для свойств RowType.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-566">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="1fc1d-567">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-568">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-569">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-569">Example</span></span>

<span data-ttu-id="1fc1d-570">В следующем примере показан элемент **EntityType** с двумя дочерними элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="1fc1d-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="1fc1d-571">Элемент PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-572">Элемент **PropertyRef** в языке SSDL ссылается на свойство, определенное в элементе EntityType, чтобы указать, что свойство будет выполнять одну из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="1fc1d-573">Быть частью первичного ключа таблицы, которую представляет **Тип EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="1fc1d-574">Для определения первичного ключа можно использовать один или несколько элементов **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="1fc1d-575">Дополнительные сведения см. в разделе Элемент Key.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="1fc1d-576">Будет зависимым или основным элементом ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="1fc1d-577">Дополнительные сведения см. в разделе Элемент ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="1fc1d-578">Элемент **PropertyRef** может иметь только следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="1fc1d-579">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-580">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="1fc1d-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-581">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-581">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-582">В следующей таблице описаны атрибуты, которые можно применить к элементу **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="1fc1d-583">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-583">Attribute Name</span></span> | <span data-ttu-id="1fc1d-584">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-584">Is Required</span></span> | <span data-ttu-id="1fc1d-585">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="1fc1d-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-586">**Name**</span></span>       | <span data-ttu-id="1fc1d-587">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-587">Yes</span></span>         | <span data-ttu-id="1fc1d-588">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="1fc1d-589">К элементу **PropertyRef** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="1fc1d-590">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="1fc1d-591">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-592">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-592">Example</span></span>

<span data-ttu-id="1fc1d-593">В следующем примере показан элемент **PropertyRef** , используемый для определения первичного ключа с помощью ссылки на свойство, определенное для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="1fc1d-594">Элемент ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-595">Элемент **ReferentialConstraint** в языке SSDL представляет ограничение внешнего ключа (также называемое ограничением ссылочной целостности) в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="1fc1d-596">Основной и зависимый концы ограничения определяются дочерними элементами Principal и Dependent соответственно.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="1fc1d-597">Столбцы, которые участвуют в основном и зависимом концах ограничения, указываются элементами PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="1fc1d-598">Элемент **ReferentialConstraint** является необязательным дочерним элементом элемента Association.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="1fc1d-599">Если элемент **ReferentialConstraint** не используется для сопоставления ограничения внешнего ключа, указанного в элементе **Association** , для этого необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="1fc1d-600">Элемент **ReferentialConstraint** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="1fc1d-601">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="1fc1d-602">Principal (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="1fc1d-603">Зависимый (ровно один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="1fc1d-604">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-605">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-605">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-606">К элементу **ReferentialConstraint** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="1fc1d-607">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-608">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-609">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-609">Example</span></span>

<span data-ttu-id="1fc1d-610">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="1fc1d-611">Элемент ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-612">Элемент **ReturnType** в языке SSDL указывает тип возвращаемого значения для функции, которая определена в элементе **Function** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="1fc1d-613">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="1fc1d-614">Тип возвращаемого значения функции указывается с помощью атрибута **Type** или элемента **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="1fc1d-615">Элемент **ReturnType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="1fc1d-616">Тип CollectionType (один)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="1fc1d-617">К элементу **ReturnType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="1fc1d-618">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-619">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-620">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-620">Example</span></span>

<span data-ttu-id="1fc1d-621">В следующем примере используется **функция** , возвращающая коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="1fc1d-622">Элемент RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-623">Элемент **RowType** в языке SSDL определяет неименованную структуру как тип возвращаемого значения для функции, определенной в хранилище.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="1fc1d-624">Элемент **RowType** является дочерним элементом элемента **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="1fc1d-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="1fc1d-625">Элемент **RowType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="1fc1d-626">Property (один или больше)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="1fc1d-627">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-627">Example</span></span>

<span data-ttu-id="1fc1d-628">В следующем примере показана функция Store, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="1fc1d-629">Элемент Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="1fc1d-630">Элемент **Schema** в языке SSDL является корневым элементом определения модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="1fc1d-631">Он содержит определения объектов, функций и контейнеров, из которых состоит модель хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="1fc1d-632">Элемент **Schema** может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="1fc1d-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="1fc1d-633">Взаимосвязь</span><span class="sxs-lookup"><span data-stu-id="1fc1d-633">Association</span></span>
-   <span data-ttu-id="1fc1d-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="1fc1d-634">EntityType</span></span>
-   <span data-ttu-id="1fc1d-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="1fc1d-635">EntityContainer</span></span>
-   <span data-ttu-id="1fc1d-636">Функция</span><span class="sxs-lookup"><span data-stu-id="1fc1d-636">Function</span></span>

<span data-ttu-id="1fc1d-637">Элемент **Schema** использует атрибут **Namespace** для определения пространства имен для типа сущности и объектов ассоциаций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="1fc1d-638">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="1fc1d-639">Пространство имен модели хранения отличается от пространства имен XML элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="1fc1d-640">Пространство имен модели хранения (как определено атрибутом **Namespace** ) является логическим контейнером для типов сущностей и типов взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="1fc1d-641">Пространство имен XML (обозначенное атрибутом **xmlns** ) элемента **Schema** является пространством имен по умолчанию для дочерних элементов и атрибутов элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="1fc1d-642">Пространства имен XML формы https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (где YYYY и MM представляют год и месяц соответственно) зарезервированы для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="1fc1d-643">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="1fc1d-644">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="1fc1d-644">Applicable Attributes</span></span>

<span data-ttu-id="1fc1d-645">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **схемы** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="1fc1d-646">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="1fc1d-646">Attribute Name</span></span>            | <span data-ttu-id="1fc1d-647">Необходимо</span><span class="sxs-lookup"><span data-stu-id="1fc1d-647">Is Required</span></span> | <span data-ttu-id="1fc1d-648">Значение</span><span class="sxs-lookup"><span data-stu-id="1fc1d-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-649">**Namespace**</span></span>             | <span data-ttu-id="1fc1d-650">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-650">Yes</span></span>         | <span data-ttu-id="1fc1d-651">Пространство имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-651">The namespace of the storage model.</span></span> <span data-ttu-id="1fc1d-652">Значение атрибута **Namespace** используется для формирования полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="1fc1d-653">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, полное имя **EntityType** — пространстве ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="1fc1d-654">Следующие строки не могут использоваться в качестве значения для атрибута **пространства имен** : **System**, **временные**или **EDM**.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="1fc1d-655">Значение атрибута **пространства имен** не может совпадать со значением атрибута **Namespace** в элементе CSDL-схемы.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="1fc1d-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-656">**Alias**</span></span>                 | <span data-ttu-id="1fc1d-657">Нет</span><span class="sxs-lookup"><span data-stu-id="1fc1d-657">No</span></span>          | <span data-ttu-id="1fc1d-658">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="1fc1d-659">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, а значение атрибута **Alias** — *Сторажемодел*, то можно использовать сторажемодел. Customer как полное имя **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="1fc1d-660">**Поставщик**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-660">**Provider**</span></span>              | <span data-ttu-id="1fc1d-661">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-661">Yes</span></span>         | <span data-ttu-id="1fc1d-662">Поставщик данных.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="1fc1d-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="1fc1d-664">Да</span><span class="sxs-lookup"><span data-stu-id="1fc1d-664">Yes</span></span>         | <span data-ttu-id="1fc1d-665">Маркер, который указывает поставщику, какой манифест должен быть возвращен.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="1fc1d-666">Формат маркера не определен.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-666">No format for the token is defined.</span></span> <span data-ttu-id="1fc1d-667">Значения для маркера определяются поставщиком.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="1fc1d-668">Дополнительные сведения о токенах манифеста поставщика SQL Server см. в разделе SqlClient для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="1fc1d-669">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-669">Example</span></span>

<span data-ttu-id="1fc1d-670">В следующем примере показан элемент **схемы** , содержащий элемент **EntityContainer** , два элемента **EntityType** и один элемент **Association** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="1fc1d-671">Атрибуты примечаний</span><span class="sxs-lookup"><span data-stu-id="1fc1d-671">Annotation Attributes</span></span>

<span data-ttu-id="1fc1d-672">Атрибуты annotation в языке SSDL представляют собой настраиваемые атрибуты XML в модели хранения, которые содержат дополнительные метаданные об элементах в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="1fc1d-673">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="1fc1d-674">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="1fc1d-675">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="1fc1d-676">К данному конкретному элементу языка SSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="1fc1d-677">Метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-678">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-678">Example</span></span>

<span data-ttu-id="1fc1d-679">В следующем примере показан элемент EntityType с атрибутом аннотации, примененным к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="1fc1d-680">В примере также показан элемент Annotation, добавленный к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="1fc1d-681">Элементы Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="1fc1d-682">Элементы annotation в языке SSDL представляют собой настраиваемые элементы XML в модели хранения, которые содержат дополнительные метаданные о модели хранения.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="1fc1d-683">Элементы annotation должны не только иметь верную структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="1fc1d-684">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="1fc1d-685">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="1fc1d-686">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="1fc1d-687">У данного элемента языка SSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="1fc1d-688">Начиная с .NET Framework версии 4 метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="1fc1d-689">Пример</span><span class="sxs-lookup"><span data-stu-id="1fc1d-689">Example</span></span>

<span data-ttu-id="1fc1d-690">В следующем примере показан элемент EntityType, имеющий элемент Annotation (**кустомелемент**).</span><span class="sxs-lookup"><span data-stu-id="1fc1d-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="1fc1d-691">В примере также показан атрибут аннотации, примененный к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="1fc1d-692">Аспекты (модель SSDL)</span><span class="sxs-lookup"><span data-stu-id="1fc1d-692">Facets (SSDL)</span></span>

<span data-ttu-id="1fc1d-693">Аспекты в языке SSDL представляют ограничения для типов столбцов, указанных в элементах Property.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="1fc1d-694">Аспекты реализуются как атрибуты XML для элементов **свойств** .</span><span class="sxs-lookup"><span data-stu-id="1fc1d-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="1fc1d-695">В следующей таблице описываются аспекты, поддерживаемые в SSDL.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="1fc1d-696">Аспект</span><span class="sxs-lookup"><span data-stu-id="1fc1d-696">Facet</span></span>           | <span data-ttu-id="1fc1d-697">Описание</span><span class="sxs-lookup"><span data-stu-id="1fc1d-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fc1d-698">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-698">**Collation**</span></span>   | <span data-ttu-id="1fc1d-699">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="1fc1d-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-700">**FixedLength**</span></span> | <span data-ttu-id="1fc1d-701">Указывает, может ли изменяться длина значения столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="1fc1d-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-702">**MaxLength**</span></span>   | <span data-ttu-id="1fc1d-703">Указывает максимальную длину значения столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="1fc1d-704">**Точность**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-704">**Precision**</span></span>   | <span data-ttu-id="1fc1d-705">Для свойств типа **Decimal**указывает количество цифр, которое может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="1fc1d-706">Для свойств типа **time**, **DateTime**и **DateTimeOffset**указывается количество цифр для дробной части секунды значения столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="1fc1d-707">**Масштаб**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-707">**Scale**</span></span>       | <span data-ttu-id="1fc1d-708">Задает число цифр справа от десятичной запятой в значении столбца.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="1fc1d-709">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="1fc1d-709">**Unicode**</span></span>     | <span data-ttu-id="1fc1d-710">Указывает, будет ли значение столбца храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="1fc1d-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
