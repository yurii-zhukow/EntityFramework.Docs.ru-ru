---
title: Связи, свойства навигации и внешние ключи - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 0cc18ee8d1b9d1633535e4b8186ffc4c68daf32b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121934"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="1765f-102">Связи, свойства навигации и внешние ключи</span><span class="sxs-lookup"><span data-stu-id="1765f-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="1765f-103">Этот раздел содержит общие сведения о том, как платформа Entity Framework управляет связи между сущностями.</span><span class="sxs-lookup"><span data-stu-id="1765f-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="1765f-104">Она также предоставляет некоторые рекомендации о том, как сопоставить и управлять связями.</span><span class="sxs-lookup"><span data-stu-id="1765f-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="1765f-105">Связи в EF</span><span class="sxs-lookup"><span data-stu-id="1765f-105">Relationships in EF</span></span>

<span data-ttu-id="1765f-106">В реляционных базах данных связи (также называемый связи) между таблицами определяются через внешние ключи.</span><span class="sxs-lookup"><span data-stu-id="1765f-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="1765f-107">Внешний ключ (FK) — столбец или сочетание столбцов, который используется для принудительного связи между данными в двух таблицах.</span><span class="sxs-lookup"><span data-stu-id="1765f-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="1765f-108">Обычно существует три типа связей: один к одному, один ко многим и многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="1765f-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="1765f-109">В отношении "один ко многим" внешний ключ определен в таблице много окончания отношения.</span><span class="sxs-lookup"><span data-stu-id="1765f-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="1765f-110">Связь многие ко многим включает в себя определением третьей таблицы (другое название таблицу соединения или соединения), первичный ключ состоит из внешних ключей из обоих связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="1765f-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="1765f-111">В взаимно-однозначной связи Кроме того, как внешний ключ действует первичный ключ и отсутствует отдельный столбец внешнего ключа, для любой из таблиц.</span><span class="sxs-lookup"><span data-stu-id="1765f-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="1765f-112">На следующем рисунке две таблицы, участвующие в связи один ко многим.</span><span class="sxs-lookup"><span data-stu-id="1765f-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="1765f-113">**Курс** таблица является зависимой таблицы, так как она содержит **DepartmentID** столбец, который связывает его с **отдел** таблицы.</span><span class="sxs-lookup"><span data-stu-id="1765f-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Database2](~/ef6/media/database2.png)

<span data-ttu-id="1765f-115">В Entity Framework сущности могут быть связаны с другими сущностями через связь или связь.</span><span class="sxs-lookup"><span data-stu-id="1765f-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="1765f-116">Каждое отношение содержит две конечные точки, описывающих тип сущности и кратность типа (один, ноль или один или множество) для двух сущностей в этой связи.</span><span class="sxs-lookup"><span data-stu-id="1765f-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="1765f-117">Связи могут регулироваться ссылочное ограничение, описывающий, какие конечных элементов отношения относится к основной роли, а какой зависимой роли.</span><span class="sxs-lookup"><span data-stu-id="1765f-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="1765f-118">Свойства навигации предоставляют способ навигации по ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="1765f-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="1765f-119">Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует.</span><span class="sxs-lookup"><span data-stu-id="1765f-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="1765f-120">Свойства навигации позволяют перейти связей и управление ими в обоих направлениях, возвращая объект ссылки (если атрибут кратности имеет один или ноль или один) или коллекции (Если кратность равна много).</span><span class="sxs-lookup"><span data-stu-id="1765f-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="1765f-121">Также можно иметь односторонней навигации в этом случае только на одном из типов, участвует в связи, а не на оба определения свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1765f-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="1765f-122">Рекомендуется, чтобы включить свойства в модели, которые сопоставляются с внешними ключами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1765f-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="1765f-123">Включение свойств внешних ключей позволяет создавать или изменять отношение, изменяя значение внешнего ключа для зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="1765f-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="1765f-124">Сопоставление такого типа называется сопоставлением на основе внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1765f-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="1765f-125">С помощью внешних ключей еще более важно при работе с отсоединенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="1765f-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="1765f-126">Обратите внимание, что при работе с 1-к-1 или 1-0... 1 связи отсутствует отдельный столбец внешнего ключа, со свойством первичного ключа выступает в качестве внешнего ключа и всегда включается в модель.</span><span class="sxs-lookup"><span data-stu-id="1765f-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="1765f-127">Если внешние ключевые столбцы не включены в модель, данные сопоставления управляется как независимый объект.</span><span class="sxs-lookup"><span data-stu-id="1765f-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="1765f-128">Отношения отслеживаются с помощью ссылки на объекты вместо свойств внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1765f-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="1765f-129">Этот тип связи называется *независимом сопоставлении*.</span><span class="sxs-lookup"><span data-stu-id="1765f-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="1765f-130">Наиболее распространенный способ изменить *независимом сопоставлении* является изменение свойства навигации, которые создаются для каждой сущности, участвующей в ассоциации.</span><span class="sxs-lookup"><span data-stu-id="1765f-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="1765f-131">В модели можно использовать один или оба типа сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="1765f-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="1765f-132">Тем не менее если у вас есть чистые связь многие ко многим, связанной с таблице соединения, которая содержит только внешние ключи, EF будет использовать независимом сопоставлении для управления таких связей многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="1765f-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="1765f-133">Ниже показана концептуальная модель, созданную с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1765f-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="1765f-134">Модель содержит две сущности, участвующие в связи один ко многим.</span><span class="sxs-lookup"><span data-stu-id="1765f-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="1765f-135">Обе эти сущности имеют свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1765f-135">Both entities have navigation properties.</span></span> <span data-ttu-id="1765f-136">**Курс** depend сущность и имеет **DepartmentID** определено свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1765f-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="1765f-138">В следующем фрагменте кода та же модель, созданную с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="1765f-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class DepartmentID
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="1765f-139">Настройка или сопоставление связей</span><span class="sxs-lookup"><span data-stu-id="1765f-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="1765f-140">Оставшуюся часть этой страницы описывается, как получить доступ и использовать данные с помощью связей.</span><span class="sxs-lookup"><span data-stu-id="1765f-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="1765f-141">Сведения о настройке связи в модели см. следующие страницы.</span><span class="sxs-lookup"><span data-stu-id="1765f-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="1765f-142">Для настройки связей в Code First, см. в разделе [заметок к данным](~/ef6/modeling/code-first/data-annotations.md) и [Fluent API — связи](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="1765f-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="1765f-143">Настройка связи с помощью Entity Framework Designer, см. в разделе [связи в конструкторе EF](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="1765f-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="1765f-144">Создание и изменение отношений</span><span class="sxs-lookup"><span data-stu-id="1765f-144">Creating and modifying relationships</span></span>

<span data-ttu-id="1765f-145">В *ассоциации внешнего ключа*, при изменении отношения состояние зависимого объекта с EntityState.Unchanged, состояние изменяется на EntityState.Modified.</span><span class="sxs-lookup"><span data-stu-id="1765f-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="1765f-146">В отношении независимых изменение отношения не обновляет состояние зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="1765f-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="1765f-147">Следующие примеры демонстрируют использование свойств внешнего ключа и свойства навигации для сопоставления связанных объектов.</span><span class="sxs-lookup"><span data-stu-id="1765f-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="1765f-148">С помощью ассоциации внешних ключей каждый из этих методов можно использовать для изменения, создания или изменения связей.</span><span class="sxs-lookup"><span data-stu-id="1765f-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="1765f-149">Для независимых сопоставлений нельзя использовать свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1765f-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="1765f-150">Присваивая новое значение свойству внешнего ключа, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="1765f-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="1765f-151">Следующий код удаляет связь, задавая внешний ключ **null**.</span><span class="sxs-lookup"><span data-stu-id="1765f-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="1765f-152">Обратите внимание, что свойство внешнего ключа должен допускать значение NULL.</span><span class="sxs-lookup"><span data-stu-id="1765f-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="1765f-153">Если ссылка находится в добавленном состоянии (в этом примере объект курса), свойство навигации ссылки не будут синхронизированы с значениями ключа нового объекта, пока не вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1765f-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="1765f-154">Синхронизация не выполняется, поскольку контекст объекта не содержит постоянных ключей для добавленных объектов, пока они не будут сохранены.</span><span class="sxs-lookup"><span data-stu-id="1765f-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="1765f-155">Если необходимо выполнить полную синхронизацию сразу же установите связь новых объектов, используйте один из следующих methods.\*</span><span class="sxs-lookup"><span data-stu-id="1765f-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="1765f-156">С помощью присваивания нового объекта свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="1765f-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="1765f-157">Следующий код создает связь между курс и `department`.</span><span class="sxs-lookup"><span data-stu-id="1765f-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="1765f-158">Если объекты присоединены к контексту, `course` также добавляется `department.Courses` коллекции и соответствующим внешним ключевое свойство на `course` объекта присваивается значение ключевого свойства отдела.</span><span class="sxs-lookup"><span data-stu-id="1765f-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="1765f-159">Чтобы удалить связь, присвойте свойству навигации `null`.</span><span class="sxs-lookup"><span data-stu-id="1765f-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="1765f-160">Если вы работаете с платформой Entity Framework, который основан на .NET 4.0, связанный элемент должен быть загружен, прежде чем ему присвоено значение null.</span><span class="sxs-lookup"><span data-stu-id="1765f-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="1765f-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="1765f-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="1765f-162">Начиная с Entity Framework 5.0, который основан на .NET 4.5, можно задать связь значение null без загрузки связанного окончания.</span><span class="sxs-lookup"><span data-stu-id="1765f-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="1765f-163">Можно также задать текущее значение NULL, используя следующий метод.</span><span class="sxs-lookup"><span data-stu-id="1765f-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="1765f-164">Путем удаления или добавления объекта в коллекцию сущностей.</span><span class="sxs-lookup"><span data-stu-id="1765f-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="1765f-165">Например, можно добавить объект типа `Course` для `department.Courses` коллекции.</span><span class="sxs-lookup"><span data-stu-id="1765f-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="1765f-166">Эта операция создает отношение между конкретным **курс** и конкретным `department`.</span><span class="sxs-lookup"><span data-stu-id="1765f-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="1765f-167">Если объекты присоединены к контекст, ссылку на подразделение и свойства внешнего ключа на **курс** будет установлен объект к соответствующему `department`.</span><span class="sxs-lookup"><span data-stu-id="1765f-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="1765f-168">С помощью `ChangeRelationshipState` метод, чтобы изменить состояние указанного отношения между двумя объектами сущностей.</span><span class="sxs-lookup"><span data-stu-id="1765f-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="1765f-169">Этот метод наиболее часто используется при работе с N-уровневые приложения и *независимом сопоставлении* (он не может использоваться с сопоставлением на основе внешнего ключа).</span><span class="sxs-lookup"><span data-stu-id="1765f-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="1765f-170">Кроме того, для использования этого метода необходимо удалить вниз до `ObjectContext`, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="1765f-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="1765f-171">В следующем примере устанавливается отношение "многие ко многим" между инструкторов и курсы.</span><span class="sxs-lookup"><span data-stu-id="1765f-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="1765f-172">Вызов `ChangeRelationshipState` и передав `EntityState.Added` параметр, позволяет `SchoolContext` знать, что добавлен связь между двумя объектами:</span><span class="sxs-lookup"><span data-stu-id="1765f-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="1765f-173">Синхронизация изменений между внешними ключами и свойств навигации</span><span class="sxs-lookup"><span data-stu-id="1765f-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="1765f-174">При изменении отношения объектов, присоединенных к контексту с помощью одного из описанных выше методов Entity Framework необходимо синхронизировать внешние ключи, ссылки и коллекции. Платформа Entity Framework автоматически управляет Эта синхронизация (также известный как связь исправление up) для сущностей POCO с прокси-серверами.</span><span class="sxs-lookup"><span data-stu-id="1765f-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="1765f-175">Дополнительные сведения см. в разделе [работа с прокси](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="1765f-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="1765f-176">При использовании сущностей POCO без прокси-объектов, необходимо убедиться в том, **DetectChanges** метод вызывается для синхронизации связанных объектов в контексте.</span><span class="sxs-lookup"><span data-stu-id="1765f-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="1765f-177">Обратите, внимание, что следующие API автоматически активирует **DetectChanges** вызова.</span><span class="sxs-lookup"><span data-stu-id="1765f-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="1765f-178">Выполнение LINQ запросы к `DbSet`</span><span class="sxs-lookup"><span data-stu-id="1765f-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="1765f-179">Загрузка связанных объектов</span><span class="sxs-lookup"><span data-stu-id="1765f-179">Loading related objects</span></span>

<span data-ttu-id="1765f-180">В Entity Framework, наиболее часто используемых свойства навигации можно используйте для загрузки сущностей, которые связаны с возвращенной сущностью посредством определенной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="1765f-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="1765f-181">Дополнительные сведения см. в разделе [загрузка связанных объектов](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="1765f-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1765f-182">В сопоставлении на основе внешнего ключа при загрузке связанного конечного элемента зависимого объекта связанный объект загружается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в памяти:</span><span class="sxs-lookup"><span data-stu-id="1765f-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="1765f-183">В независимом сопоставлении связанный конечный элемент зависимого объекта запрашивается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1765f-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="1765f-184">Тем не менее если отношение изменено и ссылочное свойство зависимого объекта указывает на разные основного объекта, который загружается в контексте объекта Entity Framework будет пытаться создать отношение, как он определен на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1765f-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="1765f-185">Управление параллелизмом</span><span class="sxs-lookup"><span data-stu-id="1765f-185">Managing concurrency</span></span>

<span data-ttu-id="1765f-186">Внешний ключ, и для независимых сопоставлений проверки параллелизма основаны на ключах сущностей и других свойствах сущностей, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="1765f-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="1765f-187">При использовании EF конструктора для создания модели, задайте `ConcurrencyMode` атрибут **фиксированной** для указания, что свойство должно проверяться для параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1765f-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="1765f-188">При использовании Code First для определения модели используйте `ConcurrencyCheck` заметки на свойства, которые будут проверяться для параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1765f-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="1765f-189">При работе в режиме Code First можно также использовать `TimeStamp` заметки, чтобы указать, что свойство должно проверяться для параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1765f-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="1765f-190">В одном классе может иметь только одно свойство метки времени.</span><span class="sxs-lookup"><span data-stu-id="1765f-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="1765f-191">Во-первых, это свойство карты кода к полю, не допускающие значения NULL в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1765f-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="1765f-192">Мы рекомендуем всегда использовать ассоциации внешнего ключа при работе с сущностями, участвующие в проверке параллелизма и разрешения.</span><span class="sxs-lookup"><span data-stu-id="1765f-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="1765f-193">Дополнительные сведения см. в разделе [обработка конфликтов параллелизма](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="1765f-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="1765f-194">Работа с перекрывающимися ключами</span><span class="sxs-lookup"><span data-stu-id="1765f-194">Working with overlapping Keys</span></span>

<span data-ttu-id="1765f-195">Перекрывающиеся ключи представляют собой составные ключи, некоторые из свойств в которых также являются частью другого ключа в сущности.</span><span class="sxs-lookup"><span data-stu-id="1765f-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="1765f-196">Для независимых сопоставлений использовать перекрывающиеся ключи нельзя.</span><span class="sxs-lookup"><span data-stu-id="1765f-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="1765f-197">Для изменения сопоставления на основе внешнего ключа, содержащей перекрывающиеся ключи, рекомендуется изменять значения внешнего ключа вместо использования ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="1765f-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
