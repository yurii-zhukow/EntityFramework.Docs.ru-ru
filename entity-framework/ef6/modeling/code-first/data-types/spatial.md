---
title: Пространственный-Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182647"
---
# <a name="spatial---code-first"></a><span data-ttu-id="80cdb-102">Пространственный Code First</span><span class="sxs-lookup"><span data-stu-id="80cdb-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="80cdb-103">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="80cdb-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="80cdb-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="80cdb-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="80cdb-105">В видео и пошаговом руководстве показано, как сопоставлять пространственные типы с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="80cdb-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="80cdb-106">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="80cdb-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="80cdb-107">В этом пошаговом руководстве будет использоваться Code First для создания новой базы данных, но можно также использовать [Code First в существующей базе данных](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="80cdb-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="80cdb-108">Поддержка пространственного типа появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="80cdb-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="80cdb-109">Обратите внимание, что для использования новых функций, таких как пространственный тип, перечисления и функции с табличным значением, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="80cdb-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="80cdb-110">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="80cdb-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="80cdb-111">Чтобы использовать пространственные типы данных, необходимо также использовать поставщик Entity Framework с поддержкой пространственного доступа.</span><span class="sxs-lookup"><span data-stu-id="80cdb-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="80cdb-112">Дополнительные сведения см. в разделе [Поддержка пространственных типов в поставщике](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="80cdb-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="80cdb-113">Существует два основных типа пространственных данных: geography и geometry.</span><span class="sxs-lookup"><span data-stu-id="80cdb-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="80cdb-114">Тип данных geography хранит данные эллиптических (например, координаты широты и долготы GPS).</span><span class="sxs-lookup"><span data-stu-id="80cdb-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="80cdb-115">Тип данных geometry представляет евклидово (плоскую) систему координат.</span><span class="sxs-lookup"><span data-stu-id="80cdb-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="80cdb-116">Просмотрите видео</span><span class="sxs-lookup"><span data-stu-id="80cdb-116">Watch the video</span></span>
<span data-ttu-id="80cdb-117">В этом видео показано, как сопоставлять пространственные типы с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="80cdb-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="80cdb-118">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="80cdb-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="80cdb-119">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="80cdb-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="80cdb-120">**Видео**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="80cdb-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="80cdb-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="80cdb-121">Pre-Requisites</span></span>

<span data-ttu-id="80cdb-122">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="80cdb-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="80cdb-123">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="80cdb-123">Set up the Project</span></span>

1.  <span data-ttu-id="80cdb-124">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="80cdb-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="80cdb-125">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="80cdb-126">В левой области щелкните **Visual C @ no__t-1**, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="80cdb-127">Введите **спатиалкодефирст** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="80cdb-128">Определение новой модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="80cdb-128">Define a New Model using Code First</span></span>

<span data-ttu-id="80cdb-129">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="80cdb-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="80cdb-130">В приведенном ниже коде определяется класс университет.</span><span class="sxs-lookup"><span data-stu-id="80cdb-130">The code below defines the University class.</span></span>

<span data-ttu-id="80cdb-131">Университет имеет свойство Location типа заданное DbGeography.</span><span class="sxs-lookup"><span data-stu-id="80cdb-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="80cdb-132">Чтобы использовать тип заданное DbGeography, необходимо добавить ссылку на сборку System. Data. Entity, а также добавить инструкцию System. Data. пространственный using.</span><span class="sxs-lookup"><span data-stu-id="80cdb-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="80cdb-133">Откройте файл Program.cs и вставьте в начало файла следующие инструкции using:</span><span class="sxs-lookup"><span data-stu-id="80cdb-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="80cdb-134">Добавьте следующее определение класса университет в файл Program.cs.</span><span class="sxs-lookup"><span data-stu-id="80cdb-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="80cdb-135">Определение производного типа DbContext</span><span class="sxs-lookup"><span data-stu-id="80cdb-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="80cdb-136">Помимо определения сущностей необходимо определить класс, производный от DbContext, и предоставляет свойства DbSet @ no__t-0TEntity @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="80cdb-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="80cdb-137">Свойства DbSet @ no__t-0TEntity @ no__t-1 позволяют контексту понять, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="80cdb-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="80cdb-138">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="80cdb-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="80cdb-139">Типы DbContext и DbSet определены в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="80cdb-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="80cdb-140">Мы добавим ссылку на эту библиотеку DLL с помощью пакета NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="80cdb-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="80cdb-141">В обозреватель решений щелкните правой кнопкой мыши имя проекта.</span><span class="sxs-lookup"><span data-stu-id="80cdb-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="80cdb-142">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="80cdb-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="80cdb-143">В диалоговом окне Управление пакетами NuGet выберите вкладку в **Интернете** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="80cdb-144">Нажмите кнопку **Установить**</span><span class="sxs-lookup"><span data-stu-id="80cdb-144">Click **Install**</span></span>

<span data-ttu-id="80cdb-145">Обратите внимание, что в дополнение к сборке EntityFramework также добавляется ссылка на сборку System. ComponentModel.</span><span class="sxs-lookup"><span data-stu-id="80cdb-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="80cdb-146">В верхней части файла Program.cs добавьте следующую инструкцию using:</span><span class="sxs-lookup"><span data-stu-id="80cdb-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="80cdb-147">В Program.cs добавьте определение контекста.</span><span class="sxs-lookup"><span data-stu-id="80cdb-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="80cdb-148">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="80cdb-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="80cdb-149">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="80cdb-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="80cdb-150">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="80cdb-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="80cdb-151">Код добавляет в контекст два новых объекта университета.</span><span class="sxs-lookup"><span data-stu-id="80cdb-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="80cdb-152">Пространственные свойства инициализируются с помощью метода заданное DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="80cdb-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="80cdb-153">Географическая точка, представленная как Веллкновнтекст, передается в метод.</span><span class="sxs-lookup"><span data-stu-id="80cdb-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="80cdb-154">Затем код сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="80cdb-154">The code then saves the data.</span></span> <span data-ttu-id="80cdb-155">Затем выполняется построение и выполнение запроса LINQ, который возвращает объект университета, где его расположение ближе всего к указанному расположению.</span><span class="sxs-lookup"><span data-stu-id="80cdb-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="80cdb-156">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="80cdb-156">Compile and run the application.</span></span> <span data-ttu-id="80cdb-157">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="80cdb-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="80cdb-158">Просмотр созданной базы данных</span><span class="sxs-lookup"><span data-stu-id="80cdb-158">View the Generated Database</span></span>

<span data-ttu-id="80cdb-159">При первом запуске приложения Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="80cdb-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="80cdb-160">Поскольку на компьютере установлен Visual Studio 2012, база данных будет создана на экземпляре LocalDB.</span><span class="sxs-lookup"><span data-stu-id="80cdb-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="80cdb-161">По умолчанию Entity Framework присваивает базе данных имя, соответствующее полному имени производного контекста (в данном примере это **спатиалкодефирст. университиконтекст**).</span><span class="sxs-lookup"><span data-stu-id="80cdb-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="80cdb-162">В последующих случаях будет использоваться существующая база данных.</span><span class="sxs-lookup"><span data-stu-id="80cdb-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="80cdb-163">Обратите внимание, что при внесении изменений в модель после создания базы данных следует использовать Code First Migrations для обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="80cdb-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="80cdb-164">Пример использования миграций см. в разделе [Code First к новой базе данных](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="80cdb-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="80cdb-165">Чтобы просмотреть базу данных и данные, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="80cdb-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="80cdb-166">В главном меню Visual Studio 2012 выберите **View** - @ no__t-2 **Обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="80cdb-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="80cdb-167">Если LocalDB отсутствует в списке серверов, щелкните правой кнопкой мыши на **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к экземпляру LocalDB.</span><span class="sxs-lookup"><span data-stu-id="80cdb-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="80cdb-168">Разверните узел LocalDB.</span><span class="sxs-lookup"><span data-stu-id="80cdb-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="80cdb-169">Unfold папку **databases** , чтобы увидеть новую базу данных, и перейдите к таблице **университетов** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="80cdb-170">Чтобы просмотреть данные, щелкните таблицу правой кнопкой мыши и выберите **Просмотр данных** .</span><span class="sxs-lookup"><span data-stu-id="80cdb-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="80cdb-171">Сводка</span><span class="sxs-lookup"><span data-stu-id="80cdb-171">Summary</span></span>

<span data-ttu-id="80cdb-172">В этом пошаговом руководстве мы рассмотрели использование пространственных типов с Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="80cdb-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
