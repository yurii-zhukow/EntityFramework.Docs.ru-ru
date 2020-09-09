---
title: Поддержка перечисления-Code First-EF6
description: Поддержка перечисления — Code First в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
uid: ef6/modeling/code-first/data-types/enums
ms.openlocfilehash: 2ed5cd73f8477469d3651ef4bf9f79ce80598ee7
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617177"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="c9fee-103">Поддержка перечисления — Code First</span><span class="sxs-lookup"><span data-stu-id="c9fee-103">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="c9fee-104">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="c9fee-104">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="c9fee-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="c9fee-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="c9fee-106">В этом видео и пошаговом руководстве показано, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="c9fee-106">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="c9fee-107">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="c9fee-107">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="c9fee-108">В этом пошаговом руководстве будет использоваться Code First для создания новой базы данных, но можно также использовать [Code First для сопоставлений с существующей базой данных](xref:ef6/modeling/code-first/workflows/existing-database).</span><span class="sxs-lookup"><span data-stu-id="c9fee-108">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](xref:ef6/modeling/code-first/workflows/existing-database).</span></span>

<span data-ttu-id="c9fee-109">Поддержка перечисления появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="c9fee-109">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="c9fee-110">Чтобы использовать новые функции, такие как перечисления, пространственные типы данных и функции, возвращающие табличное значение, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="c9fee-110">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="c9fee-111">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c9fee-111">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="c9fee-112">В Entity Framework перечисление может иметь следующие базовые типы: **Byte**, **Int16**, **Int32**, **Int64** или **SByte**.</span><span class="sxs-lookup"><span data-stu-id="c9fee-112">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="c9fee-113">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="c9fee-113">Watch the video</span></span>
<span data-ttu-id="c9fee-114">В этом видео показано, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="c9fee-114">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="c9fee-115">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="c9fee-115">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="c9fee-116">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="c9fee-116">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="c9fee-117">**Видео**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv)  |  [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="c9fee-117">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c9fee-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c9fee-118">Pre-Requisites</span></span>

<span data-ttu-id="c9fee-119">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="c9fee-119">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="c9fee-120">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="c9fee-120">Set up the Project</span></span>

1.  <span data-ttu-id="c9fee-121">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c9fee-121">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="c9fee-122">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="c9fee-123">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="c9fee-124">Введите **енумкодефирст** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-124">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="c9fee-125">Определение новой модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="c9fee-125">Define a New Model using Code First</span></span>

<span data-ttu-id="c9fee-126">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="c9fee-126">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="c9fee-127">В приведенном ниже коде определяется класс Department.</span><span class="sxs-lookup"><span data-stu-id="c9fee-127">The code below defines the Department class.</span></span>

<span data-ttu-id="c9fee-128">Код также определяет перечисление Департментнамес.</span><span class="sxs-lookup"><span data-stu-id="c9fee-128">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="c9fee-129">По умолчанию перечисление имеет тип **int** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-129">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="c9fee-130">Свойство Name класса Department имеет тип Департментнамес.</span><span class="sxs-lookup"><span data-stu-id="c9fee-130">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="c9fee-131">Откройте файл Program.cs и вставьте следующие определения классов.</span><span class="sxs-lookup"><span data-stu-id="c9fee-131">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="c9fee-132">Определение производного типа DbContext</span><span class="sxs-lookup"><span data-stu-id="c9fee-132">Define the DbContext Derived Type</span></span>

<span data-ttu-id="c9fee-133">В дополнение к определению сущностей необходимо определить класс, производный от DbContext и предоставляющий свойства DbSet&lt;TEntity&gt;.</span><span class="sxs-lookup"><span data-stu-id="c9fee-133">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="c9fee-134">Свойства DbSet&lt;TEntity&gt; позволяют контексту определить, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="c9fee-134">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="c9fee-135">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, в частности заполняет объекты данными из базы данных, отслеживает изменения и сохраняет данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c9fee-135">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="c9fee-136">Типы DbContext и DbSet определены в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="c9fee-136">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="c9fee-137">Мы добавим ссылку на эту библиотеку DLL с помощью пакета NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="c9fee-137">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="c9fee-138">В обозреватель решений щелкните правой кнопкой мыши имя проекта.</span><span class="sxs-lookup"><span data-stu-id="c9fee-138">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="c9fee-139">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="c9fee-139">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="c9fee-140">В диалоговом окне Управление пакетами NuGet выберите вкладку в **Интернете** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-140">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="c9fee-141">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="c9fee-141">Click **Install**</span></span>

<span data-ttu-id="c9fee-142">Обратите внимание, что в дополнение к сборке EntityFramework также добавляются ссылки на System. ComponentModel. и сборки System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="c9fee-142">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="c9fee-143">В верхней части файла Program.cs добавьте следующую инструкцию using:</span><span class="sxs-lookup"><span data-stu-id="c9fee-143">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="c9fee-144">В Program.cs добавьте определение контекста.</span><span class="sxs-lookup"><span data-stu-id="c9fee-144">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="c9fee-145">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="c9fee-145">Persist and Retrieve Data</span></span>

<span data-ttu-id="c9fee-146">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="c9fee-146">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="c9fee-147">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="c9fee-147">Add the following code into the Main function.</span></span> <span data-ttu-id="c9fee-148">Код добавляет новый объект Department в контекст.</span><span class="sxs-lookup"><span data-stu-id="c9fee-148">The code adds a new Department object to the context.</span></span> <span data-ttu-id="c9fee-149">Затем данные сохраняются.</span><span class="sxs-lookup"><span data-stu-id="c9fee-149">It then saves the data.</span></span> <span data-ttu-id="c9fee-150">Код также выполняет запрос LINQ, возвращающий отдел, имя которого — Департментнамес. English.</span><span class="sxs-lookup"><span data-stu-id="c9fee-150">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="c9fee-151">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c9fee-151">Compile and run the application.</span></span> <span data-ttu-id="c9fee-152">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="c9fee-152">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="c9fee-153">Просмотр созданной базы данных</span><span class="sxs-lookup"><span data-stu-id="c9fee-153">View the Generated Database</span></span>

<span data-ttu-id="c9fee-154">При первом запуске приложения Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="c9fee-154">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="c9fee-155">Поскольку на компьютере установлен Visual Studio 2012, база данных будет создана на экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c9fee-155">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="c9fee-156">По умолчанию Entity Framework имя базы данных после полного имени производного контекста (для этого примера это **енумкодефирст. енумтестконтекст**).</span><span class="sxs-lookup"><span data-stu-id="c9fee-156">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="c9fee-157">В последующих случаях будет использоваться существующая база данных.</span><span class="sxs-lookup"><span data-stu-id="c9fee-157">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="c9fee-158">Обратите внимание, что при внесении изменений в модель после создания базы данных следует использовать Code First Migrations для обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="c9fee-158">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="c9fee-159">Пример использования миграций см. в разделе [Code First к новой базе данных](xref:ef6/modeling/code-first/workflows/new-database) .</span><span class="sxs-lookup"><span data-stu-id="c9fee-159">See [Code First to a New Database](xref:ef6/modeling/code-first/workflows/new-database) for an example of using Migrations.</span></span>

<span data-ttu-id="c9fee-160">Чтобы просмотреть базу данных и данные, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c9fee-160">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="c9fee-161">В главном меню Visual Studio 2012 выберите **Просмотреть**  - &gt; **Обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="c9fee-161">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="c9fee-162">Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши на **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к экземпляру LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c9fee-162">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="c9fee-163">Разверните узел LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c9fee-163">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="c9fee-164">Unfold папку **databases** , чтобы увидеть новую базу данных, и перейдите к таблице **Department** . Обратите внимание, что Code First не создает таблицу, сопоставленную с типом перечисления.</span><span class="sxs-lookup"><span data-stu-id="c9fee-164">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="c9fee-165">Чтобы просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите **Просмотр данных** .</span><span class="sxs-lookup"><span data-stu-id="c9fee-165">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="c9fee-166">Итоги</span><span class="sxs-lookup"><span data-stu-id="c9fee-166">Summary</span></span>

<span data-ttu-id="c9fee-167">В этом пошаговом руководстве мы рассмотрели, как использовать типы Enum с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="c9fee-167">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
