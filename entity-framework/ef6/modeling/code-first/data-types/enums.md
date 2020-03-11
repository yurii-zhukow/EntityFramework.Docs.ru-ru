---
title: Поддержка перечисления-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415793"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="953c2-102">Поддержка перечисления — Code First</span><span class="sxs-lookup"><span data-stu-id="953c2-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="953c2-103">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="953c2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="953c2-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="953c2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="953c2-105">В этом видео и пошаговом руководстве показано, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="953c2-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="953c2-106">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="953c2-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="953c2-107">В этом пошаговом руководстве будет использоваться Code First для создания новой базы данных, но можно также использовать [Code First для сопоставлений с существующей базой данных](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="953c2-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="953c2-108">Поддержка перечисления появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="953c2-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="953c2-109">Чтобы использовать новые функции, такие как перечисления, пространственные типы данных и функции, возвращающие табличное значение, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="953c2-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="953c2-110">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="953c2-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="953c2-111">В Entity Framework перечисление может иметь следующие базовые типы: **Byte**, **Int16**, **Int32**, **Int64** или **SByte**.</span><span class="sxs-lookup"><span data-stu-id="953c2-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="953c2-112">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="953c2-112">Watch the video</span></span>
<span data-ttu-id="953c2-113">В этом видео показано, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="953c2-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="953c2-114">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="953c2-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="953c2-115">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="953c2-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="953c2-116">**Видео**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="953c2-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="953c2-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="953c2-117">Pre-Requisites</span></span>

<span data-ttu-id="953c2-118">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="953c2-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="953c2-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="953c2-119">Set up the Project</span></span>

1.  <span data-ttu-id="953c2-120">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="953c2-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="953c2-121">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="953c2-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="953c2-122">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="953c2-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="953c2-123">Введите **енумкодефирст** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="953c2-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="953c2-124">Определение новой модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="953c2-124">Define a New Model using Code First</span></span>

<span data-ttu-id="953c2-125">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="953c2-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="953c2-126">В приведенном ниже коде определяется класс Department.</span><span class="sxs-lookup"><span data-stu-id="953c2-126">The code below defines the Department class.</span></span>

<span data-ttu-id="953c2-127">Код также определяет перечисление Департментнамес.</span><span class="sxs-lookup"><span data-stu-id="953c2-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="953c2-128">По умолчанию перечисление имеет тип **int** .</span><span class="sxs-lookup"><span data-stu-id="953c2-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="953c2-129">Свойство Name класса Department имеет тип Департментнамес.</span><span class="sxs-lookup"><span data-stu-id="953c2-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="953c2-130">Откройте файл Program.cs и вставьте следующие определения классов.</span><span class="sxs-lookup"><span data-stu-id="953c2-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="953c2-131">Определение производного типа DbContext</span><span class="sxs-lookup"><span data-stu-id="953c2-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="953c2-132">В дополнение к определению сущностей необходимо определить класс, производный от DbContext и предоставляющий DbSet&lt;&gt; свойств.</span><span class="sxs-lookup"><span data-stu-id="953c2-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="953c2-133">Свойства DbSet&lt;ости&gt; позволяют контексту понять, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="953c2-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="953c2-134">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="953c2-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="953c2-135">Типы DbContext и DbSet определены в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="953c2-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="953c2-136">Мы добавим ссылку на эту библиотеку DLL с помощью пакета NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="953c2-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="953c2-137">В обозреватель решений щелкните правой кнопкой мыши имя проекта.</span><span class="sxs-lookup"><span data-stu-id="953c2-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="953c2-138">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="953c2-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="953c2-139">В диалоговом окне Управление пакетами NuGet выберите вкладку в **Интернете** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="953c2-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="953c2-140">Нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="953c2-140">Click **Install**</span></span>

<span data-ttu-id="953c2-141">Обратите внимание, что в дополнение к сборке EntityFramework также добавляются ссылки на System. ComponentModel. и сборки System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="953c2-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="953c2-142">В верхней части файла Program.cs добавьте следующую инструкцию using:</span><span class="sxs-lookup"><span data-stu-id="953c2-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="953c2-143">В Program.cs добавьте определение контекста.</span><span class="sxs-lookup"><span data-stu-id="953c2-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="953c2-144">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="953c2-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="953c2-145">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="953c2-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="953c2-146">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="953c2-146">Add the following code into the Main function.</span></span> <span data-ttu-id="953c2-147">Код добавляет новый объект Department в контекст.</span><span class="sxs-lookup"><span data-stu-id="953c2-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="953c2-148">Затем данные сохраняются.</span><span class="sxs-lookup"><span data-stu-id="953c2-148">It then saves the data.</span></span> <span data-ttu-id="953c2-149">Код также выполняет запрос LINQ, возвращающий отдел, имя которого — Департментнамес. English.</span><span class="sxs-lookup"><span data-stu-id="953c2-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="953c2-150">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="953c2-150">Compile and run the application.</span></span> <span data-ttu-id="953c2-151">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="953c2-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="953c2-152">Просмотр созданной базы данных</span><span class="sxs-lookup"><span data-stu-id="953c2-152">View the Generated Database</span></span>

<span data-ttu-id="953c2-153">При первом запуске приложения Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="953c2-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="953c2-154">Поскольку на компьютере установлен Visual Studio 2012, база данных будет создана на экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="953c2-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="953c2-155">По умолчанию Entity Framework имя базы данных после полного имени производного контекста (для этого примера это **енумкодефирст. енумтестконтекст**).</span><span class="sxs-lookup"><span data-stu-id="953c2-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="953c2-156">В последующих случаях будет использоваться существующая база данных.</span><span class="sxs-lookup"><span data-stu-id="953c2-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="953c2-157">Обратите внимание, что при внесении изменений в модель после создания базы данных следует использовать Code First Migrations для обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="953c2-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="953c2-158">Пример использования миграций см. в разделе [Code First к новой базе данных](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="953c2-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="953c2-159">Чтобы просмотреть базу данных и данные, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="953c2-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="953c2-160">В главном меню Visual Studio 2012 выберите **View** -&gt; **Обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="953c2-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="953c2-161">Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши на **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к экземпляру LocalDB.</span><span class="sxs-lookup"><span data-stu-id="953c2-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="953c2-162">Разверните узел LocalDB.</span><span class="sxs-lookup"><span data-stu-id="953c2-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="953c2-163">Unfold папку **databases** , чтобы увидеть новую базу данных, и перейдите к таблице **Department** . Обратите внимание, что Code First не создает таблицу, сопоставленную с типом перечисления.</span><span class="sxs-lookup"><span data-stu-id="953c2-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="953c2-164">Чтобы просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите **Просмотр данных** .</span><span class="sxs-lookup"><span data-stu-id="953c2-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="953c2-165">Сводка</span><span class="sxs-lookup"><span data-stu-id="953c2-165">Summary</span></span>

<span data-ttu-id="953c2-166">В этом пошаговом руководстве мы рассмотрели, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="953c2-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
