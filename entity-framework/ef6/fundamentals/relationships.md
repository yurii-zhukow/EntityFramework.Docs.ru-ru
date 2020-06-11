---
title: Связи, свойства навигации и внешние ключи — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 5807f7aeeb68328821cf45ac4f8c28efa17ed399
ms.sourcegitcommit: 92d54fe3702e0c92e198334da22bacb42e9842b1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84664199"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="b8a9b-102">Связи, свойства навигации и внешние ключи</span><span class="sxs-lookup"><span data-stu-id="b8a9b-102">Relationships, navigation properties, and foreign keys</span></span>

<span data-ttu-id="b8a9b-103">В этой статье приводятся общие сведения о том, как Entity Framework управляет связями между сущностями.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-103">This article gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="b8a9b-104">Кроме того, здесь приводятся некоторые рекомендации по сопоставлению и управлению связями.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="b8a9b-105">Связи в EF</span><span class="sxs-lookup"><span data-stu-id="b8a9b-105">Relationships in EF</span></span>

<span data-ttu-id="b8a9b-106">В реляционных базах данных связи (также называемые связями) между таблицами определяются через внешние ключи.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="b8a9b-107">Внешний ключ (FK) — это столбец или сочетание столбцов, которое применяется для принудительного установления связи между данными в двух таблицах.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="b8a9b-108">Обычно существует три типа связей: один к одному, один ко многим и многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="b8a9b-109">В связи «один ко многим» внешний ключ определяется в таблице, которая представляет собой множество элементов связи.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="b8a9b-110">Связь «многие ко многим» включает определение третьей таблицы (которая называется соединением или связующей таблицей), первичный ключ которых состоит из внешних ключей из обеих связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="b8a9b-111">В связи «один к одному» первичный ключ действует в качестве внешнего ключа и не имеет отдельного внешнего ключевого столбца для любой таблицы.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="b8a9b-112">На следующем рисунке показаны две таблицы, участвующие в связи «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="b8a9b-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="b8a9b-113">Таблица **Course** является зависимой таблицей, так как она содержит столбец **DepartmentID** , связывающий его с таблицей **отдела** .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Таблицы отделов и курсов](~/ef6/media/database2.png)

<span data-ttu-id="b8a9b-115">В Entity Framework сущность может быть связана с другими сущностями через ассоциацию или связь.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="b8a9b-116">Каждая связь содержит две конечные точки, описывающие тип сущности и кратность типа (один, ноль или один или несколько) для двух сущностей в этой связи.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="b8a9b-117">Отношение может управляться справочным ограничением, описывающим, какой из конечных элементов отношения относится к основной роли, а какой к зависимой роли.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="b8a9b-118">Свойства навигации обеспечивают способ навигации по ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="b8a9b-119">Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="b8a9b-120">Свойства навигации позволяют перемещать связи и управлять ими в обоих направлениях, возвращая либо ссылочный объект (если кратность — либо одна, либо нулевая или-одна), либо коллекция (если количество элементов равно многим).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="b8a9b-121">Вы также можете выбрать односторонний переход. в этом случае вы определяете свойство навигации только для одного из типов, участвующих в связи, а не для обеих.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="b8a9b-122">Рекомендуется включать в модель свойства, которые сопоставляются с внешними ключами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="b8a9b-123">Включение свойств внешних ключей позволяет создавать или изменять отношение, изменяя значение внешнего ключа для зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="b8a9b-124">Сопоставление такого типа называется сопоставлением на основе внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="b8a9b-125">Использование внешних ключей еще более важно при работе с отключенными сущностями.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="b8a9b-126">Обратите внимание, что при работе с 1-1 или 1 на-0. 1 связи нет отдельного внешнего ключевого столбца, свойство первичного ключа выступает в качестве внешнего ключа и всегда включается в модель.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="b8a9b-127">Если внешние ключевые столбцы не включены в модель, сведения о взаимосвязих управляются как независимый объект.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="b8a9b-128">Отношения отправляются через ссылки на объекты, а не на внешние ключевые свойства.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="b8a9b-129">Этот тип связи называется *независимой ассоциацией*.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="b8a9b-130">Наиболее распространенным способом изменения *независимой ассоциации* является изменение свойств навигации, создаваемых для каждой сущности, участвующей в ассоциации.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="b8a9b-131">В модели можно использовать один или оба типа сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="b8a9b-132">Однако если у вас есть отношение "многие ко многим", которое соединено таблицей соединения, содержащей только внешние ключи, EF будет использовать независимую ассоциацию для управления связью "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="b8a9b-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="b8a9b-133">На следующем рисунке показана концептуальная модель, созданная с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="b8a9b-134">Модель содержит две сущности, участвующие в связи "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="b8a9b-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="b8a9b-135">Обе сущности имеют свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-135">Both entities have navigation properties.</span></span> <span data-ttu-id="b8a9b-136">**Курс** является зависимой сущностью и имеет определенное свойство внешнего ключа **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-136">**Course** is the dependent entity and has the **DepartmentID** foreign key property defined.</span></span>

![Таблицы отделов и курсов со свойствами навигации](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="b8a9b-138">В следующем фрагменте кода показана та же модель, которая была создана с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-138">The following code snippet shows the same model that was created with Code First.</span></span>

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

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="b8a9b-139">Настройка или сопоставление связей</span><span class="sxs-lookup"><span data-stu-id="b8a9b-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="b8a9b-140">В оставшейся части этой страницы описывается, как получить доступ к данным и управлять ими с помощью связей.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="b8a9b-141">Сведения о настройке связей в модели см. на следующих страницах.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="b8a9b-142">Сведения о настройке связей в Code First см. в разделе [заметки к данным](~/ef6/modeling/code-first/data-annotations.md) и [API Fluent — связи](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="b8a9b-143">Сведения о настройке связей с помощью Entity Framework Designer см. в разделе [связи с конструктором EF](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="b8a9b-144">Создание и изменение связей</span><span class="sxs-lookup"><span data-stu-id="b8a9b-144">Creating and modifying relationships</span></span>

<span data-ttu-id="b8a9b-145">При взаимосвязи с *внешним ключом*состояние зависимого объекта с `EntityState.Unchanged` состоянием изменяется на `EntityState.Modified` .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="b8a9b-146">В независимых отношениях изменение связи не приводит к обновлению состояния зависимого объекта.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="b8a9b-147">В следующих примерах показано, как использовать свойства внешнего ключа и свойства навигации для связывания связанных объектов.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="b8a9b-148">С помощью ассоциаций внешнего ключа можно использовать любой из методов для изменения, создания или изменения связей.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="b8a9b-149">Для независимых сопоставлений нельзя использовать свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="b8a9b-150">Путем присвоения нового значения свойству внешнего ключа, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="b8a9b-151">Следующий код удаляет связь, присвоив внешнему ключу **значение NULL**.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="b8a9b-152">Обратите внимание, что свойство внешнего ключа должно допускать значение null.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="b8a9b-153">Если ссылка находится в добавленном состоянии (в данном примере это объект Course), свойство навигации ссылки не будет синхронизировано с ключевыми значениями нового объекта до тех пор, пока не будет вызван метод SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="b8a9b-154">Синхронизация не выполняется, поскольку контекст объекта не содержит постоянных ключей для добавленных объектов, пока они не будут сохранены.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="b8a9b-155">Если необходимо полностью синхронизировать новые объекты, как только вы настроили связь, используйте один из следующих методов. \*</span><span class="sxs-lookup"><span data-stu-id="b8a9b-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="b8a9b-156">С помощью присваивания нового объекта свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="b8a9b-157">Следующий код создает связь между курсом и `department` .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="b8a9b-158">Если объекты присоединены к контексту, то объект `course` также добавляется в `department.Courses` коллекцию, а соответствующее свойство внешнего ключа `course` объекта задается значением свойства ключа отдела.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="b8a9b-159">Чтобы удалить связь, задайте для свойства навигации значение `null` .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="b8a9b-160">При работе с Entity Framework, основанной на .NET 4,0, необходимо загрузить связанный элемент, прежде чем присвоить ему значение null.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="b8a9b-161">Пример.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="b8a9b-162">Начиная с Entity Framework 5,0, основанного на .NET 4,5, можно установить связь со значением NULL без загрузки связанного элемента.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="b8a9b-163">Можно также задать для текущего значения значение NULL с помощью следующего метода.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="b8a9b-164">Путем удаления или добавления объекта в коллекцию сущностей.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="b8a9b-165">Например, можно добавить объект типа `Course` в `department.Courses` коллекцию.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="b8a9b-166">Эта операция создает связь между определенным **курсом** и конкретным `department` .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="b8a9b-167">Если объекты присоединены к контексту, ссылка на отдел и свойство внешнего ключа в объекте **Course** будут установлены соответствующим образом `department` .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="b8a9b-168">С помощью `ChangeRelationshipState` метода можно изменить состояние указанной связи между двумя объектами сущностей.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="b8a9b-169">Этот метод чаще всего используется при работе с N-уровневых приложениями и *независимой ассоциацией* (его нельзя использовать с Ассоциацией внешнего ключа).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="b8a9b-170">Кроме того, чтобы использовать этот метод, необходимо раскрывающийся список `ObjectContext` , как показано в примере ниже.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="b8a9b-171">В следующем примере существует связь «многие ко многим» между преподавателями и курсами.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="b8a9b-172">При вызове `ChangeRelationshipState` метода и передаче `EntityState.Added` параметра сообщается `SchoolContext` о том, что между двумя объектами была добавлена связь.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="b8a9b-173">Обратите внимание, что при обновлении (не просто добавлении) связи необходимо удалить старую связь после добавления новой.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="b8a9b-174">Синхронизация изменений между внешними ключами и свойствами навигации</span><span class="sxs-lookup"><span data-stu-id="b8a9b-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="b8a9b-175">При изменении связи объектов, присоединенных к контексту с помощью одного из описанных выше методов, Entity Framework необходимо синхронизировать внешние ключи, ссылки и коллекции. Entity Framework автоматически управляет этой синхронизацией (также называется исправлением связи) для сущностей POCO с прокси-серверами.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="b8a9b-176">Дополнительные сведения см. в разделе [Работа с учетными записями-посредниками](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="b8a9b-177">При использовании сущностей POCO без прокси-серверов необходимо убедиться в том, что метод **DetectChanges** вызывается для синхронизации связанных объектов в контексте.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="b8a9b-178">Обратите внимание, что следующие интерфейсы API автоматически активируют вызов **DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="b8a9b-178">Note that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="b8a9b-179">Выполнение запроса LINQ к элементу`DbSet`</span><span class="sxs-lookup"><span data-stu-id="b8a9b-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="b8a9b-180">Загрузка связанных объектов</span><span class="sxs-lookup"><span data-stu-id="b8a9b-180">Loading related objects</span></span>

<span data-ttu-id="b8a9b-181">В Entity Framework свойства навигации обычно используются для загрузки сущностей, связанных с возвращаемой сущностью, с помощью заданной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-181">In Entity Framework you commonly use navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="b8a9b-182">Дополнительные сведения см. в разделе [Загрузка связанных объектов](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b8a9b-183">В сопоставлении на основе внешнего ключа при загрузке связанного конечного элемента зависимого объекта связанный объект загружается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в памяти:</span><span class="sxs-lookup"><span data-stu-id="b8a9b-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course = context.Courses.First(c => c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="b8a9b-184">В независимом сопоставлении связанный конечный элемент зависимого объекта запрашивается на основе значения внешнего ключа зависимого объекта, находящегося на момент загрузки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="b8a9b-185">Однако если связь была изменена, а ссылочное свойство зависимого объекта указывает на другой объект Principal, загруженный в контекст объекта, Entity Framework попытается создать связь, как определено на клиенте.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="b8a9b-186">Управление параллелизмом</span><span class="sxs-lookup"><span data-stu-id="b8a9b-186">Managing concurrency</span></span>

<span data-ttu-id="b8a9b-187">Как в внешних, так и в независимых ассоциациях проверки параллелизма основываются на ключах сущностей и других свойствах сущности, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="b8a9b-188">При использовании конструктора EF для создания модели присвойте `ConcurrencyMode` атрибуту значение **fixed** , чтобы указать, что свойство должно быть проверено на наличие параллелизма.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="b8a9b-189">При использовании Code First для определения модели используйте `ConcurrencyCheck` заметку для свойств, которые необходимо проверить на наличие параллелизма.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="b8a9b-190">При работе с Code First можно также использовать `TimeStamp` заметку, чтобы указать, что свойство должно проверяться на параллелизм.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="b8a9b-191">В данном классе может быть только одно свойство timestamp.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="b8a9b-192">Code First сопоставляет это свойство с полем базы данных, не допускающим значения NULL.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="b8a9b-193">При работе с сущностями, участвующими в проверке и разрешении параллелизма, рекомендуется всегда использовать связь по внешнему ключу.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="b8a9b-194">Дополнительные сведения см. в разделе [Обработка конфликтов параллелизма](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="b8a9b-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="b8a9b-195">Работа с перекрывающимися ключами</span><span class="sxs-lookup"><span data-stu-id="b8a9b-195">Working with overlapping Keys</span></span>

<span data-ttu-id="b8a9b-196">Перекрывающиеся ключи представляют собой составные ключи, некоторые из свойств в которых также являются частью другого ключа в сущности.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="b8a9b-197">Для независимых сопоставлений использовать перекрывающиеся ключи нельзя.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="b8a9b-198">Для изменения сопоставления на основе внешнего ключа, содержащей перекрывающиеся ключи, рекомендуется изменять значения внешнего ключа вместо использования ссылок на объекты.</span><span class="sxs-lookup"><span data-stu-id="b8a9b-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
