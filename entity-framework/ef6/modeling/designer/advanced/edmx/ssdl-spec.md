---
title: Спецификация языка SSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415475"
---
# <a name="ssdl-specification"></a><span data-ttu-id="e0efe-102">Спецификация SSDL</span><span class="sxs-lookup"><span data-stu-id="e0efe-102">SSDL Specification</span></span>
<span data-ttu-id="e0efe-103">SSDL представляет собой язык на основе XML, на котором описывается модель хранения в приложениях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0efe-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="e0efe-104">В приложении Entity Framework метаданные модели хранения загружаются из SSDL-файла (написанного на языке SSDL) в экземпляр класса System. Data. Metadata. EDM. StoreItemCollection и доступны с помощью методов в Класс System. Data. Metadata. EDM. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="e0efe-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="e0efe-105">Entity Framework использует метаданные модели хранения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.</span><span class="sxs-lookup"><span data-stu-id="e0efe-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="e0efe-106">Entity Framework Designer (конструктор EF) сохраняет сведения о модели хранения в EDMX-файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="e0efe-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="e0efe-107">Во время сборки Entity Designer использует сведения в EDMX-файле, чтобы создать SSDL-файл, необходимый для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="e0efe-108">Версии языка SSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="e0efe-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="e0efe-109">Версия на языке SSDL</span><span class="sxs-lookup"><span data-stu-id="e0efe-109">SSDL Version</span></span> | <span data-ttu-id="e0efe-110">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="e0efe-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="e0efe-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="e0efe-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="e0efe-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="e0efe-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="e0efe-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="e0efe-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="e0efe-114">Элемент Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-114">Association Element (SSDL)</span></span>

<span data-ttu-id="e0efe-115">Элемент **Association** в языке SSDL указывает столбцы таблицы, которые участвуют в ограничении внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="e0efe-116">Два обязательных дочерних элемента End указывают таблицы в концах сопоставления и кратность для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="e0efe-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="e0efe-117">Дополнительный элемент ReferentialConstraint указывает основной и зависимый концы сопоставления и участвующие столбцы.</span><span class="sxs-lookup"><span data-stu-id="e0efe-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="e0efe-118">Если элемент **ReferentialConstraint** отсутствует, для указания сопоставлений столбцов для ассоциации необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="e0efe-119">Элемент **Association** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-120">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-121">Конец (ровно два)</span><span class="sxs-lookup"><span data-stu-id="e0efe-121">End (exactly two)</span></span>
-   <span data-ttu-id="e0efe-122">ReferentialConstraint (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="e0efe-123">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-124">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-124">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-125">В следующей таблице описаны атрибуты, которые можно применить к элементу **Association** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="e0efe-126">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-126">Attribute Name</span></span> | <span data-ttu-id="e0efe-127">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-127">Is Required</span></span> | <span data-ttu-id="e0efe-128">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-129">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-129">**Name**</span></span>       | <span data-ttu-id="e0efe-130">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-130">Yes</span></span>         | <span data-ttu-id="e0efe-131">Имя соответствующего ограничения внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-132">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="e0efe-133">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-134">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-135">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-135">Example</span></span>

<span data-ttu-id="e0efe-136">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="e0efe-137">Элемент AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="e0efe-138">Элемент **AssociationSet** в языке SSDL представляет ограничение внешнего ключа между двумя таблицами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="e0efe-139">Столбцы таблицы, принимающие участие в ограничении внешнего ключа, указаны в элементе Association.</span><span class="sxs-lookup"><span data-stu-id="e0efe-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="e0efe-140">Элемент **Association** , соответствующий заданному элементу **AssociationSet** , задается в атрибуте **Association** элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="e0efe-141">Наборы ассоциаций SSDL сопоставляются с наборами ассоциаций CSDL с помощью элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="e0efe-142">Однако если сопоставление CSDL для заданного набора ассоциаций CSDL определено с помощью элемента ReferentialConstraint, соответствующий элемент **AssociationSetMapping** не требуется.</span><span class="sxs-lookup"><span data-stu-id="e0efe-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="e0efe-143">В этом случае, если имеется элемент **AssociationSetMapping** , определяемые им сопоставления будут переопределены элементом **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="e0efe-144">Элемент **AssociationSet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-145">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-146">End (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="e0efe-146">End (zero or two)</span></span>
-   <span data-ttu-id="e0efe-147">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-148">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-148">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-149">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="e0efe-150">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-150">Attribute Name</span></span>  | <span data-ttu-id="e0efe-151">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-151">Is Required</span></span> | <span data-ttu-id="e0efe-152">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-153">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-153">**Name**</span></span>        | <span data-ttu-id="e0efe-154">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-154">Yes</span></span>         | <span data-ttu-id="e0efe-155">Имя ограничения внешнего ключа, представленное набором ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="e0efe-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="e0efe-156">**Связке**</span><span class="sxs-lookup"><span data-stu-id="e0efe-156">**Association**</span></span> | <span data-ttu-id="e0efe-157">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-157">Yes</span></span>         | <span data-ttu-id="e0efe-158">Имя ассоциации, определяющее столбцы, которые участвуют в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-159">К элементу **AssociationSet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="e0efe-160">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-161">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-162">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-162">Example</span></span>

<span data-ttu-id="e0efe-163">В следующем примере показан элемент **AssociationSet** , представляющий ограничение внешнего ключа `FK_CustomerOrders` в базовой базе данных:</span><span class="sxs-lookup"><span data-stu-id="e0efe-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="e0efe-164">Элемент CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="e0efe-165">Элемент **CollectionType** в языке SSDL указывает, что возвращаемый функцией тип является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="e0efe-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="e0efe-166">Элемент **CollectionType** является дочерним по отношению к элементу ReturnType.</span><span class="sxs-lookup"><span data-stu-id="e0efe-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="e0efe-167">Тип коллекции задается с помощью дочернего элемента RowType:</span><span class="sxs-lookup"><span data-stu-id="e0efe-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="e0efe-168">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="e0efe-169">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-170">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-171">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-171">Example</span></span>

<span data-ttu-id="e0efe-172">В следующем примере показана функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="e0efe-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="e0efe-173">Элемент CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="e0efe-174">Элемент **CommandText** в языке SSDL является дочерним элементом элемента Function, который позволяет определить инструкцию SQL, выполняемую в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="e0efe-175">Элемент **CommandText** позволяет добавлять функции, аналогичные хранимым процедурам в базе данных, но вы определяете элемент **CommandText** в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="e0efe-176">Элемент **CommandText** не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="e0efe-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="e0efe-177">Тело элемента **CommandText** должно быть допустимой инструкцией SQL для базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="e0efe-178">К элементу **CommandText** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="e0efe-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-179">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-179">Example</span></span>

<span data-ttu-id="e0efe-180">В следующем примере показан элемент **Function** с дочерним элементом **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="e0efe-181">Предоставьте функцию **упдатепродуктинордер** как метод в контексте ObjectContext, импортировав его в концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="e0efe-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="e0efe-182">Элемент DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="e0efe-183">Элемент **DefiningQuery** в языке SSDL позволяет выполнять инструкцию SQL непосредственно в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="e0efe-184">Элемент **DefiningQuery** обычно используется как представление базы данных, но представление определяется в модели хранения, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="e0efe-185">Представление, определенное в элементе **DefiningQuery** , может быть сопоставлено с типом сущности в концептуальной модели через элемент EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="e0efe-186">Такие сопоставления предназначены только для чтения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-186">These mappings are read-only.</span></span>  

<span data-ttu-id="e0efe-187">В следующем синтаксисе языка SSDL показано объявление **набора EntitySet** , за которым следует элемент **DefiningQuery** , содержащий запрос, используемый для получения представления.</span><span class="sxs-lookup"><span data-stu-id="e0efe-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="e0efe-188">Хранимые процедуры можно использовать в Entity Framework для включения сценариев чтения и записи для представлений.</span><span class="sxs-lookup"><span data-stu-id="e0efe-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="e0efe-189"> В качестве базовой таблицы для получения данных и обработки изменений хранимыми процедурами можно использовать либо представление источника данных, либо представление Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="e0efe-190">Элемент **DefiningQuery** можно использовать для назначения Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="e0efe-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="e0efe-191">Хотя SQL Server Compact 3,5 не поддерживает хранимые процедуры, можно реализовать аналогичные функции с помощью элемента **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="e0efe-192">Этот элемент также может быть полезен при создании хранимых процедур для преодоления несоответствий между типами данных, используемых в языке программирования, и типами данных источника данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="e0efe-193">Можно написать параметр **DefiningQuery** , который принимает определенный набор параметров, а затем вызывает хранимую процедуру с другим набором параметров, например хранимую процедуру, которая удаляет данные.</span><span class="sxs-lookup"><span data-stu-id="e0efe-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="e0efe-194">Элемент Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="e0efe-195">**Зависимый** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, определяющего зависимый конец ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="e0efe-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="e0efe-196">**Зависимый** элемент указывает столбец (или столбцы) в таблице, ссылающейся на первичный ключевой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="e0efe-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="e0efe-197">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="e0efe-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="e0efe-198">Элемент Principal задает столбцы первичного ключа, на которые ссылаются столбцы, указанные в **зависимом** элементе.</span><span class="sxs-lookup"><span data-stu-id="e0efe-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="e0efe-199">**Зависимый** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-200">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="e0efe-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e0efe-201">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-202">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-202">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-203">В следующей таблице описаны атрибуты, которые можно применить к **зависимому** элементу.</span><span class="sxs-lookup"><span data-stu-id="e0efe-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="e0efe-204">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-204">Attribute Name</span></span> | <span data-ttu-id="e0efe-205">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-205">Is Required</span></span> | <span data-ttu-id="e0efe-206">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-207">**Роль**</span><span class="sxs-lookup"><span data-stu-id="e0efe-207">**Role**</span></span>       | <span data-ttu-id="e0efe-208">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-208">Yes</span></span>         | <span data-ttu-id="e0efe-209">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей ссылающийся столбец.</span><span class="sxs-lookup"><span data-stu-id="e0efe-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-210">К **зависимому** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="e0efe-211">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-212">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-213">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-213">Example</span></span>

<span data-ttu-id="e0efe-214">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e0efe-215">**Зависимый** элемент указывает столбец **CustomerID** таблицы **Order** как зависимый конец ограничения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="e0efe-216">Элемент Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="e0efe-217">Элемент **Documentation** в языке SSDL может использоваться для предоставления сведений об объекте, определенном в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="e0efe-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="e0efe-218">Элемент **Documentation** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-219">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="e0efe-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="e0efe-220">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-220">(zero or one element)</span></span>
-   <span data-ttu-id="e0efe-221">**Лонгдескриптион**: расширенное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="e0efe-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="e0efe-222">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-223">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-223">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-224">К элементу **Documentation** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="e0efe-225">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-226">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-227">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-227">Example</span></span>

<span data-ttu-id="e0efe-228">В следующем примере показан элемент **Documentation** в качестве дочернего элемента элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="e0efe-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="e0efe-229">Элемент End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-229">End Element (SSDL)</span></span>

<span data-ttu-id="e0efe-230">Элемент **End** в языке SSDL указывает таблицу и количество строк в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="e0efe-231">Элемент **End** может быть дочерним по отношению к элементу Association или элементу AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="e0efe-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="e0efe-232">В каждом из этих случаев дочерние элементы и применимые атрибуты будут различными.</span><span class="sxs-lookup"><span data-stu-id="e0efe-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="e0efe-233">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="e0efe-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="e0efe-234">Элемент **End** (в качестве дочернего элемента **ассоциации** ) указывает таблицу и число строк в конце ограничения внешнего ключа с атрибутами **тип** и **кратность** соответственно.</span><span class="sxs-lookup"><span data-stu-id="e0efe-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="e0efe-235">Элементы ограничения внешнего ключа определяются как часть ассоциации SSDL. Ассоциация SSDL должна содержать ровно два элемента.</span><span class="sxs-lookup"><span data-stu-id="e0efe-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="e0efe-236">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-237">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-238">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-239">Элементы аннотации (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="e0efe-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-240">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-240">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-241">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **Association** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="e0efe-242">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-242">Attribute Name</span></span>   | <span data-ttu-id="e0efe-243">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-243">Is Required</span></span> | <span data-ttu-id="e0efe-244">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-245">**Тип**</span><span class="sxs-lookup"><span data-stu-id="e0efe-245">**Type**</span></span>         | <span data-ttu-id="e0efe-246">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-246">Yes</span></span>         | <span data-ttu-id="e0efe-247">Полное имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="e0efe-248">**Роль**</span><span class="sxs-lookup"><span data-stu-id="e0efe-248">**Role**</span></span>         | <span data-ttu-id="e0efe-249">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-249">No</span></span>          | <span data-ttu-id="e0efe-250">Значение атрибута **Role** либо в основном, либо в зависимом элементе соответствующего элемента ReferentialConstraint (если используется).</span><span class="sxs-lookup"><span data-stu-id="e0efe-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="e0efe-251">**Кратность**</span><span class="sxs-lookup"><span data-stu-id="e0efe-251">**Multiplicity**</span></span> | <span data-ttu-id="e0efe-252">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-252">Yes</span></span>         | <span data-ttu-id="e0efe-253">**1**, **0.1**или **\*** в зависимости от числа строк, которые могут находиться в конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="e0efe-254">значение **1** указывает, что в конце ограничения внешнего ключа существует ровно одна строка.</span><span class="sxs-lookup"><span data-stu-id="e0efe-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="e0efe-255">**0.. 1** указывает, что в окончании ограничения внешнего ключа находится ноль или одна строка.</span><span class="sxs-lookup"><span data-stu-id="e0efe-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="e0efe-256">**\*** указывает, что в окончании ограничения внешнего ключа существует ноль, одна или несколько строк.</span><span class="sxs-lookup"><span data-stu-id="e0efe-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-257">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e0efe-258">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-259">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="e0efe-260">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-260">Example</span></span>

<span data-ttu-id="e0efe-261">В следующем примере показан элемент **Association** , который определяет ограничение внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e0efe-262">Значения **кратности** , заданные для каждого **конечного** элемента, указывают на то, что многие строки в таблице **Orders** можно связать со строкой в таблице **Customers** , но только одну строку в таблице **Customers** можно связать со строкой в таблице **Orders** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="e0efe-263">Кроме того, элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="e0efe-264">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="e0efe-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="e0efe-265">Элемент **End** (как дочерний элемент набора **AssociationSet** ) указывает таблицу в одном конце ограничения внешнего ключа в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="e0efe-266">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-267">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-268">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-269">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-269">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-270">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="e0efe-271">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-271">Attribute Name</span></span> | <span data-ttu-id="e0efe-272">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-272">Is Required</span></span> | <span data-ttu-id="e0efe-273">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="e0efe-274">**EntitySet**</span></span>  | <span data-ttu-id="e0efe-275">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-275">Yes</span></span>         | <span data-ttu-id="e0efe-276">Имя набора сущностей языка SSDL на конце ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="e0efe-277">**Роль**</span><span class="sxs-lookup"><span data-stu-id="e0efe-277">**Role**</span></span>       | <span data-ttu-id="e0efe-278">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-278">No</span></span>          | <span data-ttu-id="e0efe-279">Значение одного из атрибутов **роли** , заданных для одного **конечного** элемента соответствующего элемента Association.</span><span class="sxs-lookup"><span data-stu-id="e0efe-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-280">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e0efe-281">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-282">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="e0efe-283">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-283">Example</span></span>

<span data-ttu-id="e0efe-284">В следующем примере показан элемент **EntityContainer** с элементом **AssociationSet** с двумя элементами **End** :</span><span class="sxs-lookup"><span data-stu-id="e0efe-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="e0efe-285">Элемент EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="e0efe-286">Элемент **EntityContainer** в языке SSDL описывает структуру базового источника данных в Entity Framework приложении: наборы сущностей SSDL (определенные в элементах EntitySet) представляют таблицы в базе данных, типы сущностей SSDL (определенные в элементах EntityType) представляют строки в таблице, а наборы ассоциаций (определенные в элементах AssociationSet) представляют ограничения внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="e0efe-287">Контейнер сущностей модели хранения сопоставляется с контейнером сущностей концептуальной модели посредством элемента EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="e0efe-288">Элемент **EntityContainer** может иметь один или несколько элементов документации.</span><span class="sxs-lookup"><span data-stu-id="e0efe-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="e0efe-289">Если элемент **Documentation** присутствует, он должен предшествовать всем остальным дочерним элементам.</span><span class="sxs-lookup"><span data-stu-id="e0efe-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="e0efe-290">Элемент **EntityContainer** может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="e0efe-291">EntitySet</span></span>
-   <span data-ttu-id="e0efe-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="e0efe-292">AssociationSet</span></span>
-   <span data-ttu-id="e0efe-293">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="e0efe-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-294">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-294">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-295">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="e0efe-296">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-296">Attribute Name</span></span> | <span data-ttu-id="e0efe-297">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-297">Is Required</span></span> | <span data-ttu-id="e0efe-298">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-299">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-299">**Name**</span></span>       | <span data-ttu-id="e0efe-300">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-300">Yes</span></span>         | <span data-ttu-id="e0efe-301">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="e0efe-301">The name of the entity container.</span></span> <span data-ttu-id="e0efe-302">Это имя не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="e0efe-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-303">К элементу **EntityContainer** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="e0efe-304">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-305">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-306">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-306">Example</span></span>

<span data-ttu-id="e0efe-307">В следующем примере показан элемент **EntityContainer** , определяющий два набора сущностей и один набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="e0efe-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="e0efe-308">Обратите внимание, что тип сущности и имена типов ассоциаций определяются именем пространства имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="e0efe-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="e0efe-309">Элемент EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="e0efe-310">Элемент **EntitySet** в языке SSDL представляет таблицу или представление в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="e0efe-311">Элемент EntityType в языке SSDL представляет строку в таблице или представлении.</span><span class="sxs-lookup"><span data-stu-id="e0efe-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="e0efe-312">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="e0efe-313">Сопоставление между типом сущности CSDL и типом сущностиSSDL указано в элементе EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="e0efe-314">Элемент **EntitySet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-315">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-316">DefiningQuery (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-317">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="e0efe-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-318">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-318">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-319">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e0efe-320">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="e0efe-321">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="e0efe-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="e0efe-322">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-322">Attribute Name</span></span> | <span data-ttu-id="e0efe-323">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-323">Is Required</span></span> | <span data-ttu-id="e0efe-324">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-325">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-325">**Name**</span></span>       | <span data-ttu-id="e0efe-326">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-326">Yes</span></span>         | <span data-ttu-id="e0efe-327">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="e0efe-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="e0efe-328">**Типе**</span><span class="sxs-lookup"><span data-stu-id="e0efe-328">**EntityType**</span></span> | <span data-ttu-id="e0efe-329">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-329">Yes</span></span>         | <span data-ttu-id="e0efe-330">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="e0efe-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="e0efe-331">**Схема**</span><span class="sxs-lookup"><span data-stu-id="e0efe-331">**Schema**</span></span>     | <span data-ttu-id="e0efe-332">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-332">No</span></span>          | <span data-ttu-id="e0efe-333">Схема базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="e0efe-334">**Таблица**</span><span class="sxs-lookup"><span data-stu-id="e0efe-334">**Table**</span></span>      | <span data-ttu-id="e0efe-335">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-335">No</span></span>          | <span data-ttu-id="e0efe-336">Таблица базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="e0efe-337">К элементу **EntitySet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="e0efe-338">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-339">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-340">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-340">Example</span></span>

<span data-ttu-id="e0efe-341">В следующем примере показан элемент **EntityContainer** , который имеет два элемента **EntitySet** и один элемент **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="e0efe-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="e0efe-342">Элемент EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="e0efe-343">Элемент **EntityType** в языке SSDL представляет строку в таблице или представлении базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="e0efe-344">Элемент EntitySet в языке SSDL представляет таблицу или представление, в котором появляются строки.</span><span class="sxs-lookup"><span data-stu-id="e0efe-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="e0efe-345">Атрибут **EntityType** элемента **EntitySet** указывает конкретный тип сущности SSDL, представляющий строки в наборе сущностей SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="e0efe-346">Сопоставление между типом сущности SSDL и типом сущности CSDL указано в элементе EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="e0efe-347">Элемент **EntityType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-348">Документация (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-349">Key (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="e0efe-350">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="e0efe-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-351">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-351">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-352">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="e0efe-353">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-353">Attribute Name</span></span> | <span data-ttu-id="e0efe-354">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-354">Is Required</span></span> | <span data-ttu-id="e0efe-355">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-356">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-356">**Name**</span></span>       | <span data-ttu-id="e0efe-357">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-357">Yes</span></span>         | <span data-ttu-id="e0efe-358">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e0efe-358">The name of the entity type.</span></span> <span data-ttu-id="e0efe-359">Данное значение обычно совпадает с именем таблицы, в которой тип сущности представляет строку.</span><span class="sxs-lookup"><span data-stu-id="e0efe-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="e0efe-360">Это значение не может содержать точек (.).</span><span class="sxs-lookup"><span data-stu-id="e0efe-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-361">К элементу **EntityType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="e0efe-362">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-363">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-364">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-364">Example</span></span>

<span data-ttu-id="e0efe-365">В следующем примере показан элемент **EntityType** с двумя свойствами:</span><span class="sxs-lookup"><span data-stu-id="e0efe-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="e0efe-366">Элемент Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-366">Function Element (SSDL)</span></span>

<span data-ttu-id="e0efe-367">Элемент **Function** в языке SSDL указывает хранимую процедуру, которая существует в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="e0efe-368">Элемент **Function** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-369">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-370">Параметр (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="e0efe-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="e0efe-371">CommandText (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="e0efe-372">ReturnType (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="e0efe-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="e0efe-373">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="e0efe-374">Тип возвращаемого значения для функции должен быть указан либо как элемент **ReturnType** , либо как атрибут **ReturnType** (см. ниже), но не оба.</span><span class="sxs-lookup"><span data-stu-id="e0efe-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="e0efe-375">Хранимая процедура, указанная в модели хранения, может быть импортирована в концептуальную модель приложения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="e0efe-376">Дополнительные сведения см. в разделе [запросы с помощью хранимых процедур](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="e0efe-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="e0efe-377">Элемент **Function** также может использоваться для определения пользовательских функций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-378">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-378">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-379">В следующей таблице описаны атрибуты, которые можно применить к элементу **Function** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e0efe-380">Некоторые атрибуты (не перечислены здесь) могут быть дополнены псевдонимом **хранилища** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="e0efe-381">Эти атрибуты используются мастером обновления модели в ходе обновления модели.</span><span class="sxs-lookup"><span data-stu-id="e0efe-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="e0efe-382">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-382">Attribute Name</span></span>             | <span data-ttu-id="e0efe-383">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-383">Is Required</span></span> | <span data-ttu-id="e0efe-384">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-385">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-385">**Name**</span></span>                   | <span data-ttu-id="e0efe-386">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-386">Yes</span></span>         | <span data-ttu-id="e0efe-387">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="e0efe-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e0efe-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="e0efe-388">**ReturnType**</span></span>             | <span data-ttu-id="e0efe-389">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-389">No</span></span>          | <span data-ttu-id="e0efe-390">Возвращаемый тип хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="e0efe-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="e0efe-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="e0efe-391">**Aggregate**</span></span>              | <span data-ttu-id="e0efe-392">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-392">No</span></span>          | <span data-ttu-id="e0efe-393">Значение **true** , если хранимая процедура возвращает статистическое значение; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="e0efe-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="e0efe-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="e0efe-394">**BuiltIn**</span></span>                | <span data-ttu-id="e0efe-395">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-395">No</span></span>          | <span data-ttu-id="e0efe-396">**Значение true** , если функция является встроенной функцией<sup>1</sup> ; в противном случае — **false**.</span><span class="sxs-lookup"><span data-stu-id="e0efe-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="e0efe-397">**сторефунктионнаме**</span><span class="sxs-lookup"><span data-stu-id="e0efe-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="e0efe-398">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-398">No</span></span>          | <span data-ttu-id="e0efe-399">Имя хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="e0efe-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e0efe-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="e0efe-400">**NiladicFunction**</span></span>        | <span data-ttu-id="e0efe-401">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-401">No</span></span>          | <span data-ttu-id="e0efe-402">**Значение true** , если функция является функцией без параметров<sup>2</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="e0efe-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="e0efe-403">**IsComposable**</span></span>           | <span data-ttu-id="e0efe-404">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-404">No</span></span>          | <span data-ttu-id="e0efe-405">**Значение true** , если функция является составной функцией<sup>3</sup> ; В противном случае — **значение false** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="e0efe-406">**параметертипесемантикс**</span><span class="sxs-lookup"><span data-stu-id="e0efe-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="e0efe-407">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-407">No</span></span>          | <span data-ttu-id="e0efe-408">Перечисление, определяющее семантику типа, которая используется для разрешения перегрузок функций.</span><span class="sxs-lookup"><span data-stu-id="e0efe-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="e0efe-409">Перечисление определено в манифесте поставщика для определения функции.</span><span class="sxs-lookup"><span data-stu-id="e0efe-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="e0efe-410">Значение по умолчанию — **алловимплиЦитконверсион**.</span><span class="sxs-lookup"><span data-stu-id="e0efe-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="e0efe-411">**Схема**</span><span class="sxs-lookup"><span data-stu-id="e0efe-411">**Schema**</span></span>                 | <span data-ttu-id="e0efe-412">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-412">No</span></span>          | <span data-ttu-id="e0efe-413">Имя схемы, в которой определена хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="e0efe-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="e0efe-414"><sup>1</sup> встроенная функция — это функция, определенная в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="e0efe-415">Сведения о функциях, определенных в модели хранения, см. в разделе Элемент CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e0efe-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="e0efe-416"><sup>2</sup> функция без параметров не принимает никаких параметров и, при вызове, не требует круглых скобок.</span><span class="sxs-lookup"><span data-stu-id="e0efe-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="e0efe-417"><sup>3</sup> две функции являются составными, если выходные данные одной функции могут быть входными данными для другой функции.</span><span class="sxs-lookup"><span data-stu-id="e0efe-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="e0efe-418">К элементу **функции** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="e0efe-419">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-420">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-421">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-421">Example</span></span>

<span data-ttu-id="e0efe-422">В следующем примере показан элемент **Function** , соответствующий хранимой процедуре **упдатеордеркуантити** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="e0efe-423">Хранимая процедура принимает два параметра и не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="e0efe-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="e0efe-424">Элемент Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-424">Key Element (SSDL)</span></span>

<span data-ttu-id="e0efe-425">Элемент **Key** в языке SSDL представляет первичный ключ таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="e0efe-426">**Key** — это дочерний элемент элемента EntityType, который представляет строку в таблице.</span><span class="sxs-lookup"><span data-stu-id="e0efe-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="e0efe-427">Первичный ключ определяется в элементе **Key** путем ссылки на один или несколько элементов свойств, определенных для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="e0efe-428">Элемент **Key** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-429">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="e0efe-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e0efe-430">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="e0efe-430">Annotation elements</span></span>

<span data-ttu-id="e0efe-431">К элементу **Key** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="e0efe-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-432">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-432">Example</span></span>

<span data-ttu-id="e0efe-433">В следующем примере показан элемент **EntityType** с ключом, который ссылается на одно свойство:</span><span class="sxs-lookup"><span data-stu-id="e0efe-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="e0efe-434">Элемент OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="e0efe-435">Элемент **OnDelete** в языке SSDL отражает поведение базы данных при удалении строки, участвующей в ограничении внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="e0efe-436">Если для действия задано значение **CASCADE**, то строки, ссылающиеся на удаляемую строку, также будут удалены.</span><span class="sxs-lookup"><span data-stu-id="e0efe-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="e0efe-437">Если для действия задано значение **нет**, то строки, ссылающиеся на удаляемую строку, также не удаляются.</span><span class="sxs-lookup"><span data-stu-id="e0efe-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="e0efe-438">Элемент **OnDelete** является дочерним элементом конечного элемента.</span><span class="sxs-lookup"><span data-stu-id="e0efe-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="e0efe-439">Элемент **OnDelete** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-440">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-441">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-442">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-442">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-443">В следующей таблице описаны атрибуты, которые можно применить к элементу **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="e0efe-444">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-444">Attribute Name</span></span> | <span data-ttu-id="e0efe-445">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-445">Is Required</span></span> | <span data-ttu-id="e0efe-446">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-447">**Действие**</span><span class="sxs-lookup"><span data-stu-id="e0efe-447">**Action**</span></span>     | <span data-ttu-id="e0efe-448">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-448">Yes</span></span>         | <span data-ttu-id="e0efe-449">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="e0efe-449">**Cascade** or **None**.</span></span> <span data-ttu-id="e0efe-450">( **Ограничение** по значению допустимо, но имеет то же поведение, что и **None**.)</span><span class="sxs-lookup"><span data-stu-id="e0efe-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-451">К элементу **OnDelete** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="e0efe-452">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-453">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-454">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-454">Example</span></span>

<span data-ttu-id="e0efe-455">В следующем примере показан элемент **Association** , который определяет ограничение внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e0efe-456">Элемент **OnDelete** указывает, что все строки в таблице **Orders** , ссылающиеся на определенную строку в таблице **Customers** , будут удалены при удалении строки в таблице **Customers** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="e0efe-457">Элемент Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="e0efe-458">Элемент **Parameter** в языке SSDL является дочерним по отношению к элементу Function, который указывает параметры для хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="e0efe-459">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-460">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-461">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-462">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-462">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-463">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="e0efe-464">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-464">Attribute Name</span></span> | <span data-ttu-id="e0efe-465">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-465">Is Required</span></span> | <span data-ttu-id="e0efe-466">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-467">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-467">**Name**</span></span>       | <span data-ttu-id="e0efe-468">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-468">Yes</span></span>         | <span data-ttu-id="e0efe-469">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="e0efe-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="e0efe-470">**Тип**</span><span class="sxs-lookup"><span data-stu-id="e0efe-470">**Type**</span></span>       | <span data-ttu-id="e0efe-471">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-471">Yes</span></span>         | <span data-ttu-id="e0efe-472">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="e0efe-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="e0efe-473">**Режим**</span><span class="sxs-lookup"><span data-stu-id="e0efe-473">**Mode**</span></span>       | <span data-ttu-id="e0efe-474">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-474">No</span></span>          | <span data-ttu-id="e0efe-475">**В**, **out**или **InOut** в зависимости от того, является ли параметр входным, выходным или входным или выходным.</span><span class="sxs-lookup"><span data-stu-id="e0efe-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="e0efe-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e0efe-476">**MaxLength**</span></span>  | <span data-ttu-id="e0efe-477">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-477">No</span></span>          | <span data-ttu-id="e0efe-478">Максимальная длина параметра.</span><span class="sxs-lookup"><span data-stu-id="e0efe-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e0efe-479">**Точность**</span><span class="sxs-lookup"><span data-stu-id="e0efe-479">**Precision**</span></span>  | <span data-ttu-id="e0efe-480">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-480">No</span></span>          | <span data-ttu-id="e0efe-481">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="e0efe-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="e0efe-482">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="e0efe-482">**Scale**</span></span>      | <span data-ttu-id="e0efe-483">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-483">No</span></span>          | <span data-ttu-id="e0efe-484">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="e0efe-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="e0efe-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e0efe-485">**SRID**</span></span>       | <span data-ttu-id="e0efe-486">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-486">No</span></span>          | <span data-ttu-id="e0efe-487">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="e0efe-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e0efe-488">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="e0efe-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="e0efe-489">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0efe-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-490">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="e0efe-491">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-492">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-493">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-493">Example</span></span>

<span data-ttu-id="e0efe-494">В следующем примере показан элемент **Function** , имеющий два элемента **параметров** , задающих входные параметры:</span><span class="sxs-lookup"><span data-stu-id="e0efe-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="e0efe-495">Элемент Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="e0efe-496">**Основной** элемент в языке SSDL является дочерним элементом для элемента ReferentialConstraint, который определяет основной элемент ограничения внешнего ключа (также называемого ссылочным ограничением).</span><span class="sxs-lookup"><span data-stu-id="e0efe-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="e0efe-497">Элемент **Principal** указывает первичный ключевой столбец (или столбцы) в таблице, на которую ссылается другой столбец (или столбцы).</span><span class="sxs-lookup"><span data-stu-id="e0efe-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="e0efe-498">Элементы **PropertyRef** указывают, на какие столбцы ссылаются.</span><span class="sxs-lookup"><span data-stu-id="e0efe-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="e0efe-499">Зависимый элемент указывает столбцы, которые ссылаются на первичные ключевые столбцы, указанные в элементе **Principal** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="e0efe-500">Элемент **Principal** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="e0efe-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e0efe-501">PropertyRef (один или несколько)</span><span class="sxs-lookup"><span data-stu-id="e0efe-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e0efe-502">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-503">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-503">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-504">В следующей таблице описаны атрибуты, которые можно применить к **основному** элементу.</span><span class="sxs-lookup"><span data-stu-id="e0efe-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="e0efe-505">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-505">Attribute Name</span></span> | <span data-ttu-id="e0efe-506">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-506">Is Required</span></span> | <span data-ttu-id="e0efe-507">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-508">**Роль**</span><span class="sxs-lookup"><span data-stu-id="e0efe-508">**Role**</span></span>       | <span data-ttu-id="e0efe-509">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-509">Yes</span></span>         | <span data-ttu-id="e0efe-510">То же значение, что и для атрибута **Role** (если используется) соответствующего конечного элемента; в противном случае — имя таблицы, содержащей упоминаемый столбец.</span><span class="sxs-lookup"><span data-stu-id="e0efe-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-511">К **основному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="e0efe-512">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-513">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-514">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-514">Example</span></span>

<span data-ttu-id="e0efe-515">В следующем примере показан элемент Association, использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e0efe-516">Элемент **Principal** указывает столбец **CustomerID** таблицы **Customer** в качестве основного элемента ограничения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="e0efe-517">Элемент Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-517">Property Element (SSDL)</span></span>

<span data-ttu-id="e0efe-518">Элемент **Property** в языке SSDL представляет столбец в таблице в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="e0efe-519">Элементы **свойств** являются дочерними элементами типа EntityType, которые представляют строки в таблице.</span><span class="sxs-lookup"><span data-stu-id="e0efe-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="e0efe-520">Каждый элемент **Property** , определенный для элемента **EntityType** , представляет столбец.</span><span class="sxs-lookup"><span data-stu-id="e0efe-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="e0efe-521">Элемент **Свойства** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="e0efe-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-522">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-522">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-523">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="e0efe-524">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-524">Attribute Name</span></span>            | <span data-ttu-id="e0efe-525">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-525">Is Required</span></span> | <span data-ttu-id="e0efe-526">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-527">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-527">**Name**</span></span>                  | <span data-ttu-id="e0efe-528">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-528">Yes</span></span>         | <span data-ttu-id="e0efe-529">Имя соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="e0efe-530">**Тип**</span><span class="sxs-lookup"><span data-stu-id="e0efe-530">**Type**</span></span>                  | <span data-ttu-id="e0efe-531">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-531">Yes</span></span>         | <span data-ttu-id="e0efe-532">Тип соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="e0efe-533">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="e0efe-533">**Nullable**</span></span>              | <span data-ttu-id="e0efe-534">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-534">No</span></span>          | <span data-ttu-id="e0efe-535">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли соответствующий столбец иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="e0efe-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="e0efe-536">**Максимально**</span><span class="sxs-lookup"><span data-stu-id="e0efe-536">**DefaultValue**</span></span>          | <span data-ttu-id="e0efe-537">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-537">No</span></span>          | <span data-ttu-id="e0efe-538">Значение по умолчанию соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e0efe-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e0efe-539">**MaxLength**</span></span>             | <span data-ttu-id="e0efe-540">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-540">No</span></span>          | <span data-ttu-id="e0efe-541">Максимальная длина соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="e0efe-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e0efe-542">**FixedLength**</span></span>           | <span data-ttu-id="e0efe-543">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-543">No</span></span>          | <span data-ttu-id="e0efe-544">Значение **true** или **false** в зависимости от того, будет ли значение соответствующего столбца храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="e0efe-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="e0efe-545">**Точность**</span><span class="sxs-lookup"><span data-stu-id="e0efe-545">**Precision**</span></span>             | <span data-ttu-id="e0efe-546">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-546">No</span></span>          | <span data-ttu-id="e0efe-547">Точность соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="e0efe-548">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="e0efe-548">**Scale**</span></span>                 | <span data-ttu-id="e0efe-549">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-549">No</span></span>          | <span data-ttu-id="e0efe-550">Масштаб соответствующего столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="e0efe-551">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="e0efe-551">**Unicode**</span></span>               | <span data-ttu-id="e0efe-552">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-552">No</span></span>          | <span data-ttu-id="e0efe-553">Значение **true** или **false** в зависимости от того, будет ли соответствующее значение столбца храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="e0efe-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="e0efe-554">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="e0efe-554">**Collation**</span></span>             | <span data-ttu-id="e0efe-555">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-555">No</span></span>          | <span data-ttu-id="e0efe-556">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="e0efe-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e0efe-557">**SRID**</span></span>                  | <span data-ttu-id="e0efe-558">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-558">No</span></span>          | <span data-ttu-id="e0efe-559">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="e0efe-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e0efe-560">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="e0efe-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e0efe-561">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0efe-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e0efe-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="e0efe-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="e0efe-563">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-563">No</span></span>          | <span data-ttu-id="e0efe-564">**Нет**, **удостоверение** (если соответствующее значение столбца является удостоверением, созданным в базе данных) или **вычисляемым** (если соответствующее значение столбца вычислено в базе данных).</span><span class="sxs-lookup"><span data-stu-id="e0efe-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="e0efe-565">Недопустимо для свойств RowType.</span><span class="sxs-lookup"><span data-stu-id="e0efe-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-566">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="e0efe-567">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-568">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-569">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-569">Example</span></span>

<span data-ttu-id="e0efe-570">В следующем примере показан элемент **EntityType** с двумя дочерними элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="e0efe-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="e0efe-571">Элемент PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="e0efe-572">Элемент **PropertyRef** в языке SSDL ссылается на свойство, определенное в элементе EntityType, чтобы указать, что свойство будет выполнять одну из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="e0efe-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="e0efe-573">Быть частью первичного ключа таблицы, которую представляет **Тип EntityType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="e0efe-574">Для определения первичного ключа можно использовать один или несколько элементов **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="e0efe-575">Дополнительные сведения см. в разделе Элемент Key.</span><span class="sxs-lookup"><span data-stu-id="e0efe-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="e0efe-576">Будет зависимым или основным элементом ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="e0efe-577">Дополнительные сведения см. в разделе Элемент ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="e0efe-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="e0efe-578">Элемент **PropertyRef** может иметь только следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="e0efe-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="e0efe-579">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-580">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="e0efe-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-581">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-581">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-582">В следующей таблице описаны атрибуты, которые можно применить к элементу **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="e0efe-583">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-583">Attribute Name</span></span> | <span data-ttu-id="e0efe-584">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-584">Is Required</span></span> | <span data-ttu-id="e0efe-585">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="e0efe-586">**Название**</span><span class="sxs-lookup"><span data-stu-id="e0efe-586">**Name**</span></span>       | <span data-ttu-id="e0efe-587">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-587">Yes</span></span>         | <span data-ttu-id="e0efe-588">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="e0efe-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="e0efe-589">К элементу **PropertyRef** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="e0efe-590">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e0efe-591">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-592">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-592">Example</span></span>

<span data-ttu-id="e0efe-593">В следующем примере показан элемент **PropertyRef** , используемый для определения первичного ключа с помощью ссылки на свойство, определенное для элемента **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="e0efe-594">Элемент ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="e0efe-595">Элемент **ReferentialConstraint** в языке SSDL представляет ограничение внешнего ключа (также называемое ограничением ссылочной целостности) в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="e0efe-596">Основной и зависимый концы ограничения определяются дочерними элементами Principal и Dependent соответственно.</span><span class="sxs-lookup"><span data-stu-id="e0efe-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="e0efe-597">Столбцы, которые участвуют в основном и зависимом концах ограничения, указываются элементами PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="e0efe-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="e0efe-598">Элемент **ReferentialConstraint** является необязательным дочерним элементом элемента Association.</span><span class="sxs-lookup"><span data-stu-id="e0efe-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="e0efe-599">Если элемент **ReferentialConstraint** не используется для сопоставления ограничения внешнего ключа, указанного в элементе **Association** , для этого необходимо использовать элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e0efe-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="e0efe-600">Элемент **ReferentialConstraint** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="e0efe-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="e0efe-601">Документация (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e0efe-602">Principal (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="e0efe-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="e0efe-603">Зависимый (ровно один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="e0efe-604">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-605">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-605">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-606">К элементу **ReferentialConstraint** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="e0efe-607">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-608">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-609">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-609">Example</span></span>

<span data-ttu-id="e0efe-610">В следующем примере показан элемент **Association** , использующий элемент **ReferentialConstraint** для указания столбцов, участвующих в ограничении внешнего ключа **FK\_кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="e0efe-611">Элемент ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="e0efe-612">Элемент **ReturnType** в языке SSDL указывает тип возвращаемого значения для функции, которая определена в элементе **Function** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="e0efe-613">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="e0efe-614">Тип возвращаемого значения функции указывается с помощью атрибута **Type** или элемента **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="e0efe-615">Элемент **ReturnType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="e0efe-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="e0efe-616">Тип CollectionType (один)</span><span class="sxs-lookup"><span data-stu-id="e0efe-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="e0efe-617">К элементу **ReturnType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="e0efe-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="e0efe-618">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e0efe-619">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-620">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-620">Example</span></span>

<span data-ttu-id="e0efe-621">В следующем примере используется **функция** , возвращающая коллекцию строк.</span><span class="sxs-lookup"><span data-stu-id="e0efe-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="e0efe-622">Элемент RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="e0efe-623">Элемент **RowType** в языке SSDL определяет неименованную структуру как тип возвращаемого значения для функции, определенной в хранилище.</span><span class="sxs-lookup"><span data-stu-id="e0efe-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="e0efe-624">Элемент **RowType** является дочерним элементом элемента **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="e0efe-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="e0efe-625">Элемент **RowType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="e0efe-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="e0efe-626">Property (один или больше)</span><span class="sxs-lookup"><span data-stu-id="e0efe-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="e0efe-627">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-627">Example</span></span>

<span data-ttu-id="e0efe-628">В следующем примере показана функция Store, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="e0efe-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="e0efe-629">Элемент Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="e0efe-630">Элемент **Schema** в языке SSDL является корневым элементом определения модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="e0efe-631">Он содержит определения объектов, функций и контейнеров, из которых состоит модель хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="e0efe-632">Элемент **Schema** может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="e0efe-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="e0efe-633">Взаимосвязь</span><span class="sxs-lookup"><span data-stu-id="e0efe-633">Association</span></span>
-   <span data-ttu-id="e0efe-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="e0efe-634">EntityType</span></span>
-   <span data-ttu-id="e0efe-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="e0efe-635">EntityContainer</span></span>
-   <span data-ttu-id="e0efe-636">Компонент</span><span class="sxs-lookup"><span data-stu-id="e0efe-636">Function</span></span>

<span data-ttu-id="e0efe-637">Элемент **Schema** использует атрибут **Namespace** для определения пространства имен для типа сущности и объектов ассоциаций в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="e0efe-638">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="e0efe-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="e0efe-639">Пространство имен модели хранения отличается от пространства имен XML элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="e0efe-640">Пространство имен модели хранения (как определено атрибутом **Namespace** ) является логическим контейнером для типов сущностей и типов взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="e0efe-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="e0efe-641">Пространство имен XML (обозначенное атрибутом **xmlns** ) элемента **Schema** является пространством имен по умолчанию для дочерних элементов и атрибутов элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="e0efe-642">Пространства имен XML формы https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (где YYYY и MM представляют год и месяц соответственно) зарезервированы для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="e0efe-643">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="e0efe-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e0efe-644">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="e0efe-644">Applicable Attributes</span></span>

<span data-ttu-id="e0efe-645">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **схемы** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="e0efe-646">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="e0efe-646">Attribute Name</span></span>            | <span data-ttu-id="e0efe-647">Обязательный</span><span class="sxs-lookup"><span data-stu-id="e0efe-647">Is Required</span></span> | <span data-ttu-id="e0efe-648">Значение</span><span class="sxs-lookup"><span data-stu-id="e0efe-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-649">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="e0efe-649">**Namespace**</span></span>             | <span data-ttu-id="e0efe-650">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-650">Yes</span></span>         | <span data-ttu-id="e0efe-651">Пространство имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-651">The namespace of the storage model.</span></span> <span data-ttu-id="e0efe-652">Значение атрибута **Namespace** используется для формирования полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="e0efe-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="e0efe-653">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, полное имя **EntityType** — пространстве ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="e0efe-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="e0efe-654">Следующие строки не могут использоваться в качестве значения для атрибута **пространства имен** : **System**, **временные**или **EDM**.</span><span class="sxs-lookup"><span data-stu-id="e0efe-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="e0efe-655">Значение атрибута **пространства имен** не может совпадать со значением атрибута **Namespace** в элементе CSDL-схемы.</span><span class="sxs-lookup"><span data-stu-id="e0efe-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="e0efe-656">**Псевдоним**</span><span class="sxs-lookup"><span data-stu-id="e0efe-656">**Alias**</span></span>                 | <span data-ttu-id="e0efe-657">нет</span><span class="sxs-lookup"><span data-stu-id="e0efe-657">No</span></span>          | <span data-ttu-id="e0efe-658">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="e0efe-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="e0efe-659">Например, если **EntityType** с именем *Customer* находится в пространстве имен пространстве ExampleModel. Store, а значение атрибута **Alias** — *Сторажемодел*, то можно использовать сторажемодел. Customer как полное имя **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="e0efe-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="e0efe-660">**Поставщик**</span><span class="sxs-lookup"><span data-stu-id="e0efe-660">**Provider**</span></span>              | <span data-ttu-id="e0efe-661">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-661">Yes</span></span>         | <span data-ttu-id="e0efe-662">Поставщик данных.</span><span class="sxs-lookup"><span data-stu-id="e0efe-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="e0efe-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="e0efe-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="e0efe-664">Да</span><span class="sxs-lookup"><span data-stu-id="e0efe-664">Yes</span></span>         | <span data-ttu-id="e0efe-665">Маркер, который указывает поставщику, какой манифест должен быть возвращен.</span><span class="sxs-lookup"><span data-stu-id="e0efe-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="e0efe-666">Формат маркера не определен.</span><span class="sxs-lookup"><span data-stu-id="e0efe-666">No format for the token is defined.</span></span> <span data-ttu-id="e0efe-667">Значения для маркера определяются поставщиком.</span><span class="sxs-lookup"><span data-stu-id="e0efe-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="e0efe-668">Дополнительные сведения о токенах манифеста поставщика SQL Server см. в разделе SqlClient для Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0efe-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="e0efe-669">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-669">Example</span></span>

<span data-ttu-id="e0efe-670">В следующем примере показан элемент **схемы** , содержащий элемент **EntityContainer** , два элемента **EntityType** и один элемент **Association** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="e0efe-671">Атрибуты примечаний</span><span class="sxs-lookup"><span data-stu-id="e0efe-671">Annotation Attributes</span></span>

<span data-ttu-id="e0efe-672">Атрибуты annotation в языке SSDL представляют собой настраиваемые атрибуты XML в модели хранения, которые содержат дополнительные метаданные об элементах в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="e0efe-673">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="e0efe-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="e0efe-674">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="e0efe-675">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="e0efe-676">К данному конкретному элементу языка SSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="e0efe-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="e0efe-677">Метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="e0efe-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-678">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-678">Example</span></span>

<span data-ttu-id="e0efe-679">В следующем примере показан элемент EntityType с атрибутом аннотации, примененным к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="e0efe-680">В примере также показан элемент Annotation, добавленный к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="e0efe-681">Элементы Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="e0efe-682">Элементы annotation в языке SSDL представляют собой настраиваемые элементы XML в модели хранения, которые содержат дополнительные метаданные о модели хранения.</span><span class="sxs-lookup"><span data-stu-id="e0efe-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="e0efe-683">Элементы annotation должны не только иметь верную структуру XML, но и соответствовать следующим ограничениям.</span><span class="sxs-lookup"><span data-stu-id="e0efe-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="e0efe-684">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="e0efe-685">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="e0efe-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="e0efe-686">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="e0efe-687">У данного элемента языка SSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="e0efe-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="e0efe-688">Начиная с .NET Framework версии 4 метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="e0efe-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e0efe-689">Пример</span><span class="sxs-lookup"><span data-stu-id="e0efe-689">Example</span></span>

<span data-ttu-id="e0efe-690">В следующем примере показан элемент EntityType, имеющий элемент Annotation (**кустомелемент**).</span><span class="sxs-lookup"><span data-stu-id="e0efe-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="e0efe-691">В примере также показан атрибут аннотации, примененный к свойству **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="e0efe-692">Аспекты (модель SSDL)</span><span class="sxs-lookup"><span data-stu-id="e0efe-692">Facets (SSDL)</span></span>

<span data-ttu-id="e0efe-693">Аспекты в языке SSDL представляют ограничения для типов столбцов, указанных в элементах Property.</span><span class="sxs-lookup"><span data-stu-id="e0efe-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="e0efe-694">Аспекты реализуются как атрибуты XML для элементов **свойств** .</span><span class="sxs-lookup"><span data-stu-id="e0efe-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="e0efe-695">В следующей таблице описываются аспекты, поддерживаемые в SSDL.</span><span class="sxs-lookup"><span data-stu-id="e0efe-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="e0efe-696">Аспект</span><span class="sxs-lookup"><span data-stu-id="e0efe-696">Facet</span></span>           | <span data-ttu-id="e0efe-697">Description</span><span class="sxs-lookup"><span data-stu-id="e0efe-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0efe-698">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="e0efe-698">**Collation**</span></span>   | <span data-ttu-id="e0efe-699">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="e0efe-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="e0efe-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e0efe-700">**FixedLength**</span></span> | <span data-ttu-id="e0efe-701">Указывает, может ли изменяться длина значения столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="e0efe-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e0efe-702">**MaxLength**</span></span>   | <span data-ttu-id="e0efe-703">Указывает максимальную длину значения столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="e0efe-704">**Точность**</span><span class="sxs-lookup"><span data-stu-id="e0efe-704">**Precision**</span></span>   | <span data-ttu-id="e0efe-705">Для свойств типа **Decimal**указывает количество цифр, которое может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="e0efe-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="e0efe-706">Для свойств типа **time**, **DateTime**и **DateTimeOffset**указывается количество цифр для дробной части секунды значения столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="e0efe-707">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="e0efe-707">**Scale**</span></span>       | <span data-ttu-id="e0efe-708">Задает число цифр справа от десятичной запятой в значении столбца.</span><span class="sxs-lookup"><span data-stu-id="e0efe-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="e0efe-709">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="e0efe-709">**Unicode**</span></span>     | <span data-ttu-id="e0efe-710">Указывает, будет ли значение столбца храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="e0efe-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
