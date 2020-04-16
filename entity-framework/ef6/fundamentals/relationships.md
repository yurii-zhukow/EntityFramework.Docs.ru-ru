---
title: Отношения, навигационные свойства и иностранные ключи - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 76169f25d01bed81ad0748c9e85a67bd112a6e65
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434330"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="bf320-102">Отношения, навигационные свойства и иностранные ключи</span><span class="sxs-lookup"><span data-stu-id="bf320-102">Relationships, navigation properties, and foreign keys</span></span>

<span data-ttu-id="bf320-103">В этой статье приводится обзор того, как система Entity Framework управляет отношениями между сущностями.</span><span class="sxs-lookup"><span data-stu-id="bf320-103">This article gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="bf320-104">Он также дает некоторые рекомендации о том, как составить карту и манипулировать отношениями.</span><span class="sxs-lookup"><span data-stu-id="bf320-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="bf320-105">Отношения в EF</span><span class="sxs-lookup"><span data-stu-id="bf320-105">Relationships in EF</span></span>

<span data-ttu-id="bf320-106">В реляционных базах данных отношения (также называемые ассоциациями) между таблицами определяются через иностранные ключи.</span><span class="sxs-lookup"><span data-stu-id="bf320-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="bf320-107">Внешний ключ (FK) — это столбец или сочетание столбцов, которое применяется для принудительного установления связи между данными в двух таблицах.</span><span class="sxs-lookup"><span data-stu-id="bf320-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="bf320-108">Есть, как правило, три типа отношений: один-к-одному, один-к-многим, и многие-к-многим.</span><span class="sxs-lookup"><span data-stu-id="bf320-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="bf320-109">В отношениях между собой один к многим, иностранный ключ определяется на столе, который представляет собой много конца отношений.</span><span class="sxs-lookup"><span data-stu-id="bf320-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="bf320-110">Взаимоотношения между многими и многими включают определение третьей таблицы (называемой соединением или таблицей соединения), основной ключ которой состоит из иностранных ключей из обеих связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="bf320-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="bf320-111">В отношениях один на один основной ключ действует дополнительно как иностранный ключ, и для любой из таблиц нет отдельной колонки иностранного ключа.</span><span class="sxs-lookup"><span data-stu-id="bf320-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="bf320-112">На следующем изображении показаны две таблицы, которые участвуют в отношениях между двумя другими.</span><span class="sxs-lookup"><span data-stu-id="bf320-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="bf320-113">Таблица **Курса** является зависимой таблицей, поскольку она содержит столбец **DepartmentID,** который связывает ее с таблицей **Департамента.**</span><span class="sxs-lookup"><span data-stu-id="bf320-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Таблицы департамента и курса](~/ef6/media/database2.png)

<span data-ttu-id="bf320-115">В рамках сущности сущность может быть связана с другими сущностями через ассоциацию или отношения.</span><span class="sxs-lookup"><span data-stu-id="bf320-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="bf320-116">Каждое отношение содержит два конца, описывающих тип сущности и многообразие типа (один, ноль или один, или много) для двух сущностей в этом отношении.</span><span class="sxs-lookup"><span data-stu-id="bf320-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="bf320-117">Отношение может управляться справочным ограничением, описывающим, какой из конечных элементов отношения относится к основной роли, а какой к зависимой роли.</span><span class="sxs-lookup"><span data-stu-id="bf320-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="bf320-118">Свойства навигации обеспечивают способ навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="bf320-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="bf320-119">Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует.</span><span class="sxs-lookup"><span data-stu-id="bf320-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="bf320-120">Свойства навигации позволяют перемещаться и управлять отношениями в обоих направлениях, возвращая либо эталонный объект (если разносторонность либо один или ноль или один) или коллекции (если многообразие много).</span><span class="sxs-lookup"><span data-stu-id="bf320-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="bf320-121">Вы также можете иметь одностороннюю навигацию, и в этом случае вы определяете свойство навигации только по одному из типов, которые участвуют в отношениях, а не на обоих.</span><span class="sxs-lookup"><span data-stu-id="bf320-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="bf320-122">Рекомендуется включить свойства в модель, которая отображает сявречки в иностранные ключи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bf320-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="bf320-123">Включение свойств внешних ключей позволяет создавать или изменять отношение, изменяя значение внешнего ключа для зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="bf320-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="bf320-124">Сопоставление такого типа называется сопоставлением на основе внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="bf320-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="bf320-125">Использование иностранных ключей является еще более важным при работе с отключенными объектами.</span><span class="sxs-lookup"><span data-stu-id="bf320-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="bf320-126">Обратите внимание, что при работе с 1-к-1 или 1-к-0.. 1 отношения, нет отдельной иностранной ключевой колонки, основной ключ собственности выступает в качестве иностранного ключа и всегда входит в модель.</span><span class="sxs-lookup"><span data-stu-id="bf320-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="bf320-127">Когда иностранные ключевые столбцы не включены в модель, информация об ассоциации управляется как независимый объект.</span><span class="sxs-lookup"><span data-stu-id="bf320-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="bf320-128">Отношения отслеживаются с помощью ссылок на объекты, а не с иностранными ключевыми свойствами.</span><span class="sxs-lookup"><span data-stu-id="bf320-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="bf320-129">Этот тип ассоциации называется *независимой ассоциацией.*</span><span class="sxs-lookup"><span data-stu-id="bf320-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="bf320-130">Наиболее распространенным способом изменения *независимой ассоциации* является изменение свойств навигации, которые генерируются для каждой организации, участвуют в ассоциации.</span><span class="sxs-lookup"><span data-stu-id="bf320-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="bf320-131">В модели можно использовать один или оба типа сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="bf320-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="bf320-132">Однако, если у вас есть чистые отношения между многими и многими, которые связаны таблицей соединения, содержащей только иностранные ключи, EF будет использовать независимую ассоциацию для управления такими отношениями, которые так много-к-многим.</span><span class="sxs-lookup"><span data-stu-id="bf320-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="bf320-133">Следующее изображение показывает концептуальную модель, которая была создана с помощью конструктора рамок Entity.</span><span class="sxs-lookup"><span data-stu-id="bf320-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="bf320-134">Модель содержит два сущности, которые участвуют в отношениях от одного к многим.</span><span class="sxs-lookup"><span data-stu-id="bf320-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="bf320-135">Оба объекта имеют навигационные свойства.</span><span class="sxs-lookup"><span data-stu-id="bf320-135">Both entities have navigation properties.</span></span> <span data-ttu-id="bf320-136">**Курс** является зависимым лицом и имеет **DepartmentID** иностранной ключевой собственности определяется.</span><span class="sxs-lookup"><span data-stu-id="bf320-136">**Course** is the dependent entity and has the **DepartmentID** foreign key property defined.</span></span>

![Таблицы отдела и курса с навигационными свойствами](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="bf320-138">На следующем фрагменте кода показана та же модель, которая была создана с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="bf320-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="bf320-139">Настройка или отображение отношений</span><span class="sxs-lookup"><span data-stu-id="bf320-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="bf320-140">Остальная часть этой страницы охватывает, как получить доступ и манипулировать данными с помощью отношений.</span><span class="sxs-lookup"><span data-stu-id="bf320-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="bf320-141">Для получения информации о настройке отношений в модели см.</span><span class="sxs-lookup"><span data-stu-id="bf320-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="bf320-142">Для настройки отношений в Код есначала см. [Аннотации данных](~/ef6/modeling/code-first/data-annotations.md) и [API Fluent API - Отношения.](~/ef6/modeling/code-first/fluent/relationships.md)</span><span class="sxs-lookup"><span data-stu-id="bf320-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="bf320-143">Для настройки отношений с помощью конструктора рамок Entity [см.](~/ef6/modeling/designer/relationships.md)</span><span class="sxs-lookup"><span data-stu-id="bf320-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="bf320-144">Создание и изменение отношений</span><span class="sxs-lookup"><span data-stu-id="bf320-144">Creating and modifying relationships</span></span>

<span data-ttu-id="bf320-145">В *иностранной ключевой ассоциации,* когда вы меняете отношения, состояние зависимого объекта с состоянием `EntityState.Unchanged` меняется на `EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="bf320-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="bf320-146">В независимом отношении изменение отношения не обновляет состояние зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="bf320-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="bf320-147">Ниже приведены приведения примеров использования чужих ключевых свойств и навигационных свойств для ассоциации связанных объектов.</span><span class="sxs-lookup"><span data-stu-id="bf320-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="bf320-148">С иностранными ключевыми ассоциациями можно использовать любой метод для изменения, создания или изменения отношений.</span><span class="sxs-lookup"><span data-stu-id="bf320-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="bf320-149">Для независимых сопоставлений нельзя использовать свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="bf320-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="bf320-150">Присваивая новую стоимость иностранной ключевой собственности, как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="bf320-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="bf320-151">Следующий код удаляет связь, установив иностранный ключ **на нуле.**</span><span class="sxs-lookup"><span data-stu-id="bf320-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="bf320-152">Обратите внимание, что иностранное ключевое свойство должно быть аннулировано.</span><span class="sxs-lookup"><span data-stu-id="bf320-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="bf320-153">Если ссылка находится в добавленном состоянии (в этом примере объект курса), свойство ориентировочного навигационного свойства не будет синхронизировано с ключевыми значениями нового объекта до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bf320-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="bf320-154">Синхронизация не выполняется, поскольку контекст объекта не содержит постоянных ключей для добавленных объектов, пока они не будут сохранены.</span><span class="sxs-lookup"><span data-stu-id="bf320-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="bf320-155">Если необходимо полностью синхронизировать новые объекты, как только вы установите связь, используйте один из следующих методов».</span><span class="sxs-lookup"><span data-stu-id="bf320-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="bf320-156">С помощью присваивания нового объекта свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="bf320-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="bf320-157">Следующий код создает связь между `department`курсом и .</span><span class="sxs-lookup"><span data-stu-id="bf320-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="bf320-158">Если объекты присоединены к `course` контексту, то `department.Courses` также добавляется в коллекцию, а соответствующее свойство иностранного ключа на `course` объекте устанавливается к ключевой стоимости свойства отдела.</span><span class="sxs-lookup"><span data-stu-id="bf320-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="bf320-159">Чтобы удалить связь, установите `null`свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="bf320-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="bf320-160">Если вы работаете с системой Entity Framework, основанной на .NET 4.0, то связанный конец должен быть загружен, прежде чем установить его на нулевую.</span><span class="sxs-lookup"><span data-stu-id="bf320-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="bf320-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="bf320-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="bf320-162">Начиная с entity Framework 5.0, которая основана на .NET 4.5, можно установить связь с нуля без загрузки связанного конца.</span><span class="sxs-lookup"><span data-stu-id="bf320-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="bf320-163">Можно также установить текущее значение, чтобы свести к нулю, используя следующий метод.</span><span class="sxs-lookup"><span data-stu-id="bf320-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="bf320-164">Путем удаления или добавления объекта в коллекцию сущностей.</span><span class="sxs-lookup"><span data-stu-id="bf320-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="bf320-165">Например, можно добавить объект `Course` типа `department.Courses` в коллекцию.</span><span class="sxs-lookup"><span data-stu-id="bf320-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="bf320-166">Эта операция создает связь между определенным `department` **курсом** и конкретным.</span><span class="sxs-lookup"><span data-stu-id="bf320-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="bf320-167">Если объекты присоединены к контексту, ссылка отдела **course** и иностранное ключевое `department`свойство на объекте курса будут настроены на соответствующий .</span><span class="sxs-lookup"><span data-stu-id="bf320-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="bf320-168">Используя `ChangeRelationshipState` метод для изменения состояния заданного отношения между двумя объектами сущности.</span><span class="sxs-lookup"><span data-stu-id="bf320-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="bf320-169">Этот метод чаще всего используется при работе с приложениями N-Tier и *независимой ассоциацией* (он не может быть использован с иностранной ключевой ассоциацией).</span><span class="sxs-lookup"><span data-stu-id="bf320-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="bf320-170">Кроме того, чтобы использовать этот `ObjectContext`метод вы должны упасть до , как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="bf320-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="bf320-171">В следующем примере существует много-к-много отношений между инструкторами и курсами.</span><span class="sxs-lookup"><span data-stu-id="bf320-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="bf320-172">Вызов `ChangeRelationshipState` метода и `EntityState.Added` прохождение `SchoolContext` параметра позволяет узнать, что связь была добавлена между двумя объектами:</span><span class="sxs-lookup"><span data-stu-id="bf320-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="bf320-173">Обратите внимание, что если вы обновляете (а не просто добавляете) отношения, необходимо удалить старые отношения после добавления нового:</span><span class="sxs-lookup"><span data-stu-id="bf320-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="bf320-174">Синхронизация изменений между иностранными ключами и навигационными свойствами</span><span class="sxs-lookup"><span data-stu-id="bf320-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="bf320-175">При изменении отношения объектов, прикрепленных к контексту, с помощью одного из описанных выше методов, системе entity необходимо синхронизировать иностранные ключи, ссылки и коллекции. Система сущности автоматически управляет этой синхронизацией (также известной как исправление отношений) для сущностей POCO с прокси-клиентами.</span><span class="sxs-lookup"><span data-stu-id="bf320-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="bf320-176">Для получения дополнительной информации [см.](~/ef6/fundamentals/proxies.md)</span><span class="sxs-lookup"><span data-stu-id="bf320-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="bf320-177">Если вы используете сущности POCO без прокси-технологий, необходимо убедиться, что метод **DetectChanges** вызывается для синхронизации связанных объектов в контексте.</span><span class="sxs-lookup"><span data-stu-id="bf320-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="bf320-178">Обратите внимание, что следующие AIS автоматически выпускают вызов **DetectChanges.**</span><span class="sxs-lookup"><span data-stu-id="bf320-178">Note that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="bf320-179">Выполнение запроса LIN'а против`DbSet`</span><span class="sxs-lookup"><span data-stu-id="bf320-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="bf320-180">Загрузка связанных объектов</span><span class="sxs-lookup"><span data-stu-id="bf320-180">Loading related objects</span></span>

<span data-ttu-id="bf320-181">В рамках сущности обычно используются навигационные свойства для загрузки сущностей, связанных с возвращенной сущностью определенной ассоциацией.</span><span class="sxs-lookup"><span data-stu-id="bf320-181">In Entity Framework you commonly use navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="bf320-182">Для получения дополнительной информации [см.](~/ef6/querying/related-data.md)</span><span class="sxs-lookup"><span data-stu-id="bf320-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bf320-183">В сопоставлении на основе внешнего ключа при загрузке связанного конечного элемента зависимого объекта связанный объект загружается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в памяти:</span><span class="sxs-lookup"><span data-stu-id="bf320-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c => c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="bf320-184">В независимом сопоставлении связанный конечный элемент зависимого объекта запрашивается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bf320-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="bf320-185">Однако, если связь была изменена, а свойство ссылки на зависимом объекте указывает на другой основной объект, загруженный в контексте объекта, рамочная сущность попытается создать связь так, как она определена на клиенте.</span><span class="sxs-lookup"><span data-stu-id="bf320-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="bf320-186">Управление параллелизмом</span><span class="sxs-lookup"><span data-stu-id="bf320-186">Managing concurrency</span></span>

<span data-ttu-id="bf320-187">Как в иностранных ключевых, так и в независимых ассоциациях проверки параллелизма основаны на ключах сущности и других свойствах сущности, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="bf320-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="bf320-188">При использовании EF Designer для создания `ConcurrencyMode` модели установите **атрибут, фиксированный,** чтобы указать, что свойство должно быть проверено на наличие параллелизма.</span><span class="sxs-lookup"><span data-stu-id="bf320-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="bf320-189">При использовании Code First для `ConcurrencyCheck` определения модели используйте аннотацию на свойствах, которые вы хотите проверить на наличие параллелизма.</span><span class="sxs-lookup"><span data-stu-id="bf320-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="bf320-190">При работе с Code First `TimeStamp` вы также можете использовать аннотацию, чтобы указать, что свойство должно быть проверено на наличие параллелизма.</span><span class="sxs-lookup"><span data-stu-id="bf320-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="bf320-191">Вы можете иметь только одно свойство метки времени в данном типе.</span><span class="sxs-lookup"><span data-stu-id="bf320-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="bf320-192">Code First отображает это свойство в поле, не являваещееся недействительной в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bf320-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="bf320-193">Мы рекомендуем вам всегда использовать иностранную ключевую ассоциацию при работе с организациями, которые участвуют в проверке параллелизма и разрешении.</span><span class="sxs-lookup"><span data-stu-id="bf320-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="bf320-194">Для получения дополнительной информации [см.](~/ef6/saving/concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="bf320-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="bf320-195">Работа с перекрывающимися ключами</span><span class="sxs-lookup"><span data-stu-id="bf320-195">Working with overlapping Keys</span></span>

<span data-ttu-id="bf320-196">Перекрывающиеся ключи представляют собой составные ключи, некоторые из свойств в которых также являются частью другого ключа в сущности.</span><span class="sxs-lookup"><span data-stu-id="bf320-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="bf320-197">Для независимых сопоставлений использовать перекрывающиеся ключи нельзя.</span><span class="sxs-lookup"><span data-stu-id="bf320-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="bf320-198">Для изменения сопоставления на основе внешнего ключа, содержащей перекрывающиеся ключи, рекомендуется изменять значения внешнего ключа вместо использования ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="bf320-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
