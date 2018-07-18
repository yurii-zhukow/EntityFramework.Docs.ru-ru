---
title: Поддержка перечислений — Code First — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
caps.latest.revision: 3
ms.openlocfilehash: 698c84a9f0679c67a086574de2f253f214fb8d0b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122566"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="002bd-102">Поддержка перечислений — Code First</span><span class="sxs-lookup"><span data-stu-id="002bd-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="002bd-103">**EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="002bd-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="002bd-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="002bd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="002bd-105">Это видео и пошаговые пошаговом руководстве показано, как использовать типы перечисления с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="002bd-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="002bd-106">Также демонстрируется использование перечислений в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="002bd-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="002bd-107">В этом пошаговом руководстве будет использовать Code First для создания новой базы данных, но можно также использовать [Code First для сопоставления существующей базы данных](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="002bd-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="002bd-108">В Entity Framework 5 добавлена поддержка перечисления.</span><span class="sxs-lookup"><span data-stu-id="002bd-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="002bd-109">Чтобы использовать новые функции, например перечисления, Пространственные типы данных и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="002bd-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="002bd-110">Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="002bd-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="002bd-111">В Entity Framework, что перечисление может иметь следующие базовые типы: **байтов**, **Int16**, **Int32**, **Int64** , или **SByte**.</span><span class="sxs-lookup"><span data-stu-id="002bd-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="002bd-112">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="002bd-112">Watch the video</span></span>
<span data-ttu-id="002bd-113">В этом видео показано, как использовать типы перечисления с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="002bd-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="002bd-114">Также демонстрируется использование перечислений в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="002bd-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="002bd-115">**Представленный**: Юлия Корнич</span><span class="sxs-lookup"><span data-stu-id="002bd-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="002bd-116">**Видео**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="002bd-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="002bd-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="002bd-117">Pre-Requisites</span></span>

<span data-ttu-id="002bd-118">Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="002bd-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="002bd-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="002bd-119">Set up the Project</span></span>

1.  <span data-ttu-id="002bd-120">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="002bd-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="002bd-121">На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**</span><span class="sxs-lookup"><span data-stu-id="002bd-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="002bd-122">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона</span><span class="sxs-lookup"><span data-stu-id="002bd-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="002bd-123">Введите **EnumCodeFirst** как имя проекта и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="002bd-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="002bd-124">Определения новой модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="002bd-124">Define a New Model using Code First</span></span>

<span data-ttu-id="002bd-125">При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).</span><span class="sxs-lookup"><span data-stu-id="002bd-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="002bd-126">Приведенный ниже код определяет класс отдела.</span><span class="sxs-lookup"><span data-stu-id="002bd-126">The code below defines the Department class.</span></span>

<span data-ttu-id="002bd-127">Код также определяет перечисление DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="002bd-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="002bd-128">По умолчанию имеет перечисления **int** типа.</span><span class="sxs-lookup"><span data-stu-id="002bd-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="002bd-129">Свойство Name класса Department имеет тип DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="002bd-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="002bd-130">Откройте файл Program.cs и вставьте следующие определения класса.</span><span class="sxs-lookup"><span data-stu-id="002bd-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="002bd-131">Определить тип, производный от DbContext</span><span class="sxs-lookup"><span data-stu-id="002bd-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="002bd-132">Кроме определения сущностей, необходимо определить класс, который является производным от DbContext и DbSet предоставляет&lt;TEntity&gt; свойства.</span><span class="sxs-lookup"><span data-stu-id="002bd-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="002bd-133">DbSet&lt;TEntity&gt; свойства let контекст знать, какие типы, которые вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="002bd-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="002bd-134">Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="002bd-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="002bd-135">Типы DbContext и DbSet определенные в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="002bd-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="002bd-136">Мы добавим ссылку на эту библиотеку DLL с помощью пакета EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="002bd-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="002bd-137">В обозревателе решений щелкните правой кнопкой мыши имя проекта.</span><span class="sxs-lookup"><span data-stu-id="002bd-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="002bd-138">Выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="002bd-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="002bd-139">В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета.</span><span class="sxs-lookup"><span data-stu-id="002bd-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="002bd-140">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="002bd-140">Click **Install**</span></span>

<span data-ttu-id="002bd-141">Обратите внимание, что помимо сборки EntityFramework, ссылки на сборки System.ComponentModel.DataAnnotations и System.Data.Entity добавляются также.</span><span class="sxs-lookup"><span data-stu-id="002bd-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="002bd-142">В верхней части файла Program.cs, добавьте следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="002bd-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="002bd-143">В Program.cs добавьте определение контекста.</span><span class="sxs-lookup"><span data-stu-id="002bd-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="002bd-144">Сохранения и извлечения данных</span><span class="sxs-lookup"><span data-stu-id="002bd-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="002bd-145">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="002bd-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="002bd-146">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="002bd-146">Add the following code into the Main function.</span></span> <span data-ttu-id="002bd-147">Код добавляет новый объект отдела в контекст.</span><span class="sxs-lookup"><span data-stu-id="002bd-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="002bd-148">Затем она сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="002bd-148">It then saves the data.</span></span> <span data-ttu-id="002bd-149">Код также выполняется запрос LINQ, возвращающий подразделение, где имя задается DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="002bd-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="002bd-150">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="002bd-150">Compile and run the application.</span></span> <span data-ttu-id="002bd-151">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="002bd-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="002bd-152">Представление создаваемой базы данных</span><span class="sxs-lookup"><span data-stu-id="002bd-152">View the Generated Database</span></span>

<span data-ttu-id="002bd-153">При запуске приложение в первый раз, Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="002bd-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="002bd-154">Поскольку у нас есть установленной Visual Studio 2012, базе данных создается на экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="002bd-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="002bd-155">По умолчанию Entity Framework содержит имя базы данных после полное имя производного контекста (например, **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="002bd-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="002bd-156">Последующие случаи, когда будет использоваться существующая база данных.</span><span class="sxs-lookup"><span data-stu-id="002bd-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="002bd-157">Обратите внимание, что если внести изменения в модель после создания базы данных, необходимо использовать Code First Migrations для обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="002bd-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="002bd-158">См. в разделе [Code First для новой базы данных](~/ef6/modeling/code-first/workflows/new-database.md) пример с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="002bd-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="002bd-159">Чтобы просмотреть базы данных и данных, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="002bd-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="002bd-160">В главном меню Visual Studio 2012 выберите **представление**  - &gt; **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="002bd-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="002bd-161">Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к Экземпляр LocalDB</span><span class="sxs-lookup"><span data-stu-id="002bd-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="002bd-162">Разверните узел LocalDB</span><span class="sxs-lookup"><span data-stu-id="002bd-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="002bd-163">Unfold **баз данных** папку для новой базы данных см. в разделе и перейдите к **отдел** таблицы Обратите внимание, что Code First не создает таблицу, которая сопоставляется с типом перечисления</span><span class="sxs-lookup"><span data-stu-id="002bd-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="002bd-164">Чтобы просмотреть данные, щелкните правой кнопкой мыши по таблице и выберите **Просмотр данных**</span><span class="sxs-lookup"><span data-stu-id="002bd-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="002bd-165">Сводка</span><span class="sxs-lookup"><span data-stu-id="002bd-165">Summary</span></span>

<span data-ttu-id="002bd-166">В этом пошаговом руководстве мы рассмотрели способы использования типов перечисления с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="002bd-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
