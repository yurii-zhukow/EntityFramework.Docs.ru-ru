---
title: Пространственные - EF6 Code First.
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122598"
---
# <a name="spatial---code-first"></a><span data-ttu-id="1eae8-102">Пространственные - Code First</span><span class="sxs-lookup"><span data-stu-id="1eae8-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="1eae8-103">**EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="1eae8-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="1eae8-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="1eae8-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1eae8-105">Видео и пошаговые руководства показано, как сопоставить пространственных типов с помощью Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="1eae8-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="1eae8-106">Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="1eae8-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="1eae8-107">В этом пошаговом руководстве будет использовать Code First для создания новой базы данных, но можно также использовать [Code First для существующей базы данных](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="1eae8-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="1eae8-108">В Entity Framework 5 появилась поддержка пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="1eae8-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="1eae8-109">Обратите внимание на то, что чтобы использовать новые функции, например пространственного типа, перечислимые типы и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="1eae8-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="1eae8-110">Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1eae8-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="1eae8-111">Для использования пространственных типов данных необходимо также использовать с поставщиком Entity Framework с поддержкой пространственных.</span><span class="sxs-lookup"><span data-stu-id="1eae8-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="1eae8-112">См. в разделе [поддержка пространственных типов](~/ef6/fundamentals/providers/spatial-support.md) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="1eae8-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="1eae8-113">Существует два типа основных пространственных данных: geography и geometry.</span><span class="sxs-lookup"><span data-stu-id="1eae8-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="1eae8-114">Тип данных сохраняет эллиптические данные (например, GPS координаты широты и долготы).</span><span class="sxs-lookup"><span data-stu-id="1eae8-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="1eae8-115">Тип данных geometry представляет Евклидовой (плоской) системе координат.</span><span class="sxs-lookup"><span data-stu-id="1eae8-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1eae8-116">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="1eae8-116">Watch the video</span></span>
<span data-ttu-id="1eae8-117">В этом видео показано, как сопоставить пространственных типов с помощью Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="1eae8-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="1eae8-118">Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="1eae8-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="1eae8-119">**Представленный**: Юлия Корнич</span><span class="sxs-lookup"><span data-stu-id="1eae8-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="1eae8-120">**Видео**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="1eae8-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1eae8-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1eae8-121">Pre-Requisites</span></span>

<span data-ttu-id="1eae8-122">Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="1eae8-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1eae8-123">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="1eae8-123">Set up the Project</span></span>

1.  <span data-ttu-id="1eae8-124">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1eae8-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="1eae8-125">На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**</span><span class="sxs-lookup"><span data-stu-id="1eae8-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="1eae8-126">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона</span><span class="sxs-lookup"><span data-stu-id="1eae8-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="1eae8-127">Введите **SpatialCodeFirst** как имя проекта и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="1eae8-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="1eae8-128">Определения новой модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="1eae8-128">Define a New Model using Code First</span></span>

<span data-ttu-id="1eae8-129">При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).</span><span class="sxs-lookup"><span data-stu-id="1eae8-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="1eae8-130">Приведенный ниже код определяет класс университета.</span><span class="sxs-lookup"><span data-stu-id="1eae8-130">The code below defines the University class.</span></span>

<span data-ttu-id="1eae8-131">В университете ведется свойство Location типа DbGeography.</span><span class="sxs-lookup"><span data-stu-id="1eae8-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="1eae8-132">Чтобы использовать тип DbGeography, необходимо добавить ссылку на сборку System.Data.Entity и также добавить System.Data.Spatial, с помощью инструкции.</span><span class="sxs-lookup"><span data-stu-id="1eae8-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="1eae8-133">Откройте файл Program.cs и вставьте следующие операторы using в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="1eae8-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="1eae8-134">Добавьте следующее определение класса университета в файл Program.cs.</span><span class="sxs-lookup"><span data-stu-id="1eae8-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="1eae8-135">Определить тип, производный от DbContext</span><span class="sxs-lookup"><span data-stu-id="1eae8-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="1eae8-136">Кроме определения сущностей, необходимо определить класс, который является производным от DbContext и DbSet предоставляет&lt;TEntity&gt; свойства.</span><span class="sxs-lookup"><span data-stu-id="1eae8-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="1eae8-137">DbSet&lt;TEntity&gt; свойства let контекст знать, какие типы, которые вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="1eae8-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="1eae8-138">Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1eae8-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="1eae8-139">Типы DbContext и DbSet определенные в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="1eae8-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="1eae8-140">Мы добавим ссылку на эту библиотеку DLL с помощью пакета EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="1eae8-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="1eae8-141">В обозревателе решений щелкните правой кнопкой мыши имя проекта.</span><span class="sxs-lookup"><span data-stu-id="1eae8-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="1eae8-142">Выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="1eae8-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="1eae8-143">В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета.</span><span class="sxs-lookup"><span data-stu-id="1eae8-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="1eae8-144">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="1eae8-144">Click **Install**</span></span>

<span data-ttu-id="1eae8-145">Обратите внимание, что помимо сборки EntityFramework, ссылку на сборку System.ComponentModel.DataAnnotations также добавляется.</span><span class="sxs-lookup"><span data-stu-id="1eae8-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="1eae8-146">В верхней части файла Program.cs, добавьте следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="1eae8-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="1eae8-147">В Program.cs добавьте определение контекста.</span><span class="sxs-lookup"><span data-stu-id="1eae8-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="1eae8-148">Сохранения и извлечения данных</span><span class="sxs-lookup"><span data-stu-id="1eae8-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="1eae8-149">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="1eae8-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="1eae8-150">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="1eae8-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="1eae8-151">Код добавляет два новых университета объектов к контексту.</span><span class="sxs-lookup"><span data-stu-id="1eae8-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="1eae8-152">Пространственные свойства инициализируются с помощью метода DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="1eae8-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="1eae8-153">Географическая точка представить в виде WellKnownText передается в метод.</span><span class="sxs-lookup"><span data-stu-id="1eae8-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="1eae8-154">Затем код сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="1eae8-154">The code then saves the data.</span></span> <span data-ttu-id="1eae8-155">Затем запрос LINQ, который возвращает объект университета, где она будет находиться в указанное расположение, ближайшее составлением и выполнением.</span><span class="sxs-lookup"><span data-stu-id="1eae8-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="1eae8-156">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="1eae8-156">Compile and run the application.</span></span> <span data-ttu-id="1eae8-157">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="1eae8-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="1eae8-158">Представление создаваемой базы данных</span><span class="sxs-lookup"><span data-stu-id="1eae8-158">View the Generated Database</span></span>

<span data-ttu-id="1eae8-159">При запуске приложение в первый раз, Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="1eae8-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="1eae8-160">Поскольку у нас есть установленной Visual Studio 2012, базе данных создается на экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="1eae8-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="1eae8-161">По умолчанию Entity Framework содержит имя базы данных после полное имя производного контекста (в этом примере, **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="1eae8-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="1eae8-162">Последующие случаи, когда будет использоваться существующая база данных.</span><span class="sxs-lookup"><span data-stu-id="1eae8-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="1eae8-163">Обратите внимание, что если внести изменения в модель после создания базы данных, необходимо использовать Code First Migrations для обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="1eae8-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="1eae8-164">См. в разделе [Code First для новой базы данных](~/ef6/modeling/code-first/workflows/new-database.md) пример с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="1eae8-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="1eae8-165">Чтобы просмотреть базы данных и данных, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="1eae8-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="1eae8-166">В главном меню Visual Studio 2012 выберите **представление**  - &gt; **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1eae8-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="1eae8-167">Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к экземпляр LocalDB</span><span class="sxs-lookup"><span data-stu-id="1eae8-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="1eae8-168">Разверните узел LocalDB</span><span class="sxs-lookup"><span data-stu-id="1eae8-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="1eae8-169">Unfold **баз данных** папку для новой базы данных см. в разделе и перейдите к **университеты** таблицы</span><span class="sxs-lookup"><span data-stu-id="1eae8-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="1eae8-170">Чтобы просмотреть данные, щелкните правой кнопкой мыши по таблице и выберите **Просмотр данных**</span><span class="sxs-lookup"><span data-stu-id="1eae8-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="1eae8-171">Сводка</span><span class="sxs-lookup"><span data-stu-id="1eae8-171">Summary</span></span>

<span data-ttu-id="1eae8-172">В этом пошаговом руководстве мы рассмотрели способы использования пространственных типов с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="1eae8-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
