---
title: Глоссарий Entity Framework — EF6
description: Глоссарий Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: 19d5e9e3a480337c2bcb93be5f989cc622b67dad
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620198"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="1fdef-103">Глоссарий Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1fdef-103">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="1fdef-104">Code First</span><span class="sxs-lookup"><span data-stu-id="1fdef-104">Code First</span></span>
<span data-ttu-id="1fdef-105">Создание модели Entity Framework с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="1fdef-105">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="1fdef-106">Модель может ориентироваться на существующую базу данных или новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="1fdef-106">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="1fdef-107">Контекст</span><span class="sxs-lookup"><span data-stu-id="1fdef-107">Context</span></span>
<span data-ttu-id="1fdef-108">Класс, представляющий сеанс с базой данных, позволяющий запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="1fdef-108">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="1fdef-109">Контекст является производным от класса DbContext или ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="1fdef-109">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="1fdef-110">Соглашение (Code First)</span><span class="sxs-lookup"><span data-stu-id="1fdef-110">Convention (Code First)</span></span>
<span data-ttu-id="1fdef-111">Правило, которое Entity Framework использует, чтобы вывести форму модели из классов.</span><span class="sxs-lookup"><span data-stu-id="1fdef-111">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="1fdef-112">Database First</span><span class="sxs-lookup"><span data-stu-id="1fdef-112">Database First</span></span>
<span data-ttu-id="1fdef-113">Создание модели Entity Framework с помощью конструктора EF, нацеленного на существующую базу данных.</span><span class="sxs-lookup"><span data-stu-id="1fdef-113">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="1fdef-114">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="1fdef-114">Eager loading</span></span>
<span data-ttu-id="1fdef-115">Шаблон загрузки связанных данных, при котором запрос для одного типа сущности также загружает связанные сущности в составе запроса.</span><span class="sxs-lookup"><span data-stu-id="1fdef-115">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="1fdef-116">Конструктор EF</span><span class="sxs-lookup"><span data-stu-id="1fdef-116">EF Designer</span></span>
<span data-ttu-id="1fdef-117">Визуальный конструктор в Visual Studio, позволяющий создавать Entity Frameworkную модель с помощью прямоугольников и линий.</span><span class="sxs-lookup"><span data-stu-id="1fdef-117">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="1fdef-118">Сущность</span><span class="sxs-lookup"><span data-stu-id="1fdef-118">Entity</span></span>
<span data-ttu-id="1fdef-119">Класс или объект, представляющий данные приложения, такие как клиенты, продукты и заказы.</span><span class="sxs-lookup"><span data-stu-id="1fdef-119">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="1fdef-120">EDM (модель данных с использованием сущностей)</span><span class="sxs-lookup"><span data-stu-id="1fdef-120">Entity Data Model</span></span>
<span data-ttu-id="1fdef-121">Модель, описывающая сущности и связи между ними.</span><span class="sxs-lookup"><span data-stu-id="1fdef-121">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="1fdef-122">EF использует EDM для описания концептуальной модели, относительно которой программы разработчика.</span><span class="sxs-lookup"><span data-stu-id="1fdef-122">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="1fdef-123">EDM строится на модели отношений сущностей, представленной Dr. Питер Чен.</span><span class="sxs-lookup"><span data-stu-id="1fdef-123">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="1fdef-124">Изначально модель EDM была разработана с основной целью стать общей моделью данных в наборе технологий разработки и сервера от корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="1fdef-124">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="1fdef-125">EDM также используется как часть протокола OData.</span><span class="sxs-lookup"><span data-stu-id="1fdef-125">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="1fdef-126">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="1fdef-126">Explicit loading</span></span>
<span data-ttu-id="1fdef-127">Шаблон загрузки связанных данных, при котором связанные объекты загружаются путем вызова API.</span><span class="sxs-lookup"><span data-stu-id="1fdef-127">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1fdef-128">Текучий API</span><span class="sxs-lookup"><span data-stu-id="1fdef-128">Fluent API</span></span>
<span data-ttu-id="1fdef-129">API, который можно использовать для настройки модели Code First.</span><span class="sxs-lookup"><span data-stu-id="1fdef-129">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="1fdef-130">Связь по внешнему ключу</span><span class="sxs-lookup"><span data-stu-id="1fdef-130">Foreign key association</span></span>
<span data-ttu-id="1fdef-131">Ассоциация между сущностями, в которой свойство, представляющее внешний ключ, включается в класс зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="1fdef-131">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="1fdef-132">Например, Product содержит свойство CategoryId.</span><span class="sxs-lookup"><span data-stu-id="1fdef-132">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="1fdef-133">Идентификация связи</span><span class="sxs-lookup"><span data-stu-id="1fdef-133">Identifying relationship</span></span>
<span data-ttu-id="1fdef-134">Связь, в которой первичный ключ основной сущности входит в состав первичного ключа зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="1fdef-134">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="1fdef-135">При такой связи зависимая сущность не может существовать без основной сущности.</span><span class="sxs-lookup"><span data-stu-id="1fdef-135">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="1fdef-136">Независимая ассоциация</span><span class="sxs-lookup"><span data-stu-id="1fdef-136">Independent association</span></span>
<span data-ttu-id="1fdef-137">Ассоциация между сущностями, в которой отсутствует свойство, представляющее внешний ключ в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="1fdef-137">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="1fdef-138">Например, класс Product содержит связь с категорией, но не имеет свойства CategoryId.</span><span class="sxs-lookup"><span data-stu-id="1fdef-138">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="1fdef-139">Entity Framework отслеживает состояние связи независимо от состояния сущностей на двух концах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="1fdef-139">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1fdef-140">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="1fdef-140">Lazy loading</span></span>
<span data-ttu-id="1fdef-141">Шаблон загрузки связанных данных, в котором связанные объекты автоматически загружаются при обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="1fdef-141">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="1fdef-142">Model First</span><span class="sxs-lookup"><span data-stu-id="1fdef-142">Model First</span></span>
<span data-ttu-id="1fdef-143">Создание модели Entity Framework с помощью конструктора EF, который затем используется для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1fdef-143">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="1fdef-144">Свойство навигации</span><span class="sxs-lookup"><span data-stu-id="1fdef-144">Navigation property</span></span>
<span data-ttu-id="1fdef-145">Свойство сущности, которое ссылается на другую сущность.</span><span class="sxs-lookup"><span data-stu-id="1fdef-145">A property of an entity that references another entity.</span></span> <span data-ttu-id="1fdef-146">Например, элемент Product содержит свойство навигации по категориям, а категория содержит свойство навигации Products.</span><span class="sxs-lookup"><span data-stu-id="1fdef-146">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="1fdef-147">POCO</span><span class="sxs-lookup"><span data-stu-id="1fdef-147">POCO</span></span>
<span data-ttu-id="1fdef-148">Акроним для обычного старого объекта CLR.</span><span class="sxs-lookup"><span data-stu-id="1fdef-148">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="1fdef-149">Простой класс пользователя, не имеющий зависимостей ни с одной платформой.</span><span class="sxs-lookup"><span data-stu-id="1fdef-149">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="1fdef-150">В контексте EF класс сущностей, который не является производным от EntityObject, реализует любые интерфейсы или несет любые атрибуты, определенные в EF.</span><span class="sxs-lookup"><span data-stu-id="1fdef-150">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="1fdef-151">Такие классы сущностей, отделенные от платформы сохраняемости, также называются "сохраняемостью игнорирующих".</span><span class="sxs-lookup"><span data-stu-id="1fdef-151">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="1fdef-152">Обратная связь</span><span class="sxs-lookup"><span data-stu-id="1fdef-152">Relationship inverse</span></span>
<span data-ttu-id="1fdef-153">Противоположный конец связи, например Product. Категория и Категория. Продукта.</span><span class="sxs-lookup"><span data-stu-id="1fdef-153">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="1fdef-154">Самостоятельная сущность</span><span class="sxs-lookup"><span data-stu-id="1fdef-154">Self-tracking entity</span></span>
<span data-ttu-id="1fdef-155">Сущность, построенная на основе шаблона создания кода, которая помогает при разработке N-уровневой разработки.</span><span class="sxs-lookup"><span data-stu-id="1fdef-155">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="1fdef-156">Тип "Таблица — конкретный" (TPC)</span><span class="sxs-lookup"><span data-stu-id="1fdef-156">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="1fdef-157">Метод сопоставления наследования, в котором каждый неабстрактный тип в иерархии сопоставлен с отдельной таблицей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fdef-157">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="1fdef-158">Одна таблица на иерархию (подтаблица)</span><span class="sxs-lookup"><span data-stu-id="1fdef-158">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="1fdef-159">Метод сопоставления наследования, в котором все типы в иерархии сопоставляются с одной и той же таблицей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1fdef-159">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="1fdef-160">Столбцы дискриминатора используются для того, чтобы определить тип, с которым связана каждая строка.</span><span class="sxs-lookup"><span data-stu-id="1fdef-160">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="1fdef-161">Таблица на тип (TPT)</span><span class="sxs-lookup"><span data-stu-id="1fdef-161">Table-per-type (TPT)</span></span>
<span data-ttu-id="1fdef-162">Метод сопоставления наследования, в котором общие свойства всех типов в иерархии сопоставляются с одной и той же таблицей в базе данных, но свойства, уникальные для каждого типа, сопоставляются с отдельной таблицей.</span><span class="sxs-lookup"><span data-stu-id="1fdef-162">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="1fdef-163">Обнаружение типов</span><span class="sxs-lookup"><span data-stu-id="1fdef-163">Type discovery</span></span>
<span data-ttu-id="1fdef-164">Процесс определения типов, которые должны быть частью модели Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1fdef-164">The process of identifying the types that should be part of an Entity Framework model.</span></span>
