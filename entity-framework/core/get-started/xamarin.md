---
title: Начало работы с EF Core и Xamarin — EF Core
author: codemillmatt
ms.date: 07/07/2020
ms.author: masoucou
uid: core/get-started/xamarin
ms.openlocfilehash: 14fe61e0ddd648f824bfe3c1c91a58af91b1c2d4
ms.sourcegitcommit: d85263b5d5d665dbaf94de8832e2917bce048b34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2020
ms.locfileid: "86452997"
---
# <a name="getting-started-with-ef-core-and-xamarin"></a><span data-ttu-id="c576a-102">Начало работы с EF Core и Xamarin</span><span class="sxs-lookup"><span data-stu-id="c576a-102">Getting Started with EF Core and Xamarin</span></span>

<span data-ttu-id="c576a-103">В этом руководстве вы создадите приложение [Xamarin.Forms](/xamarin/get-started/what-is-xamarin-forms), которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c576a-103">In this tutorial, you create a [Xamarin.Forms](/xamarin/get-started/what-is-xamarin-forms) application that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="c576a-104">Инструкции из этого руководства можно выполнять с помощью Visual Studio в Windows или Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="c576a-104">You can follow the tutorial by using Visual Studio on Windows or Visual Studio for Mac.</span></span>

> [!TIP]
> <span data-ttu-id="c576a-105">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Xamarin) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="c576a-105">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Xamarin).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c576a-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c576a-106">Prerequisites</span></span>

<span data-ttu-id="c576a-107">Установите одно из следующих приложений:</span><span class="sxs-lookup"><span data-stu-id="c576a-107">Install one of the below:</span></span>

* <span data-ttu-id="c576a-108">[Visual Studio 2019 версии 16.3 или более поздней](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:</span><span class="sxs-lookup"><span data-stu-id="c576a-108">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this workload:</span></span>
  * <span data-ttu-id="c576a-109">**Разработка мобильных приложений на .NET**</span><span class="sxs-lookup"><span data-stu-id="c576a-109">**Mobile Development with .NET**</span></span>
* [<span data-ttu-id="c576a-110">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="c576a-110">Visual Studio for Mac</span></span>](https://visualstudio.microsoft.com/vs/mac/)

<span data-ttu-id="c576a-111">В этой [документации содержатся подробные пошаговые инструкции по установке](/xamarin/get-started/installation) для каждой платформы.</span><span class="sxs-lookup"><span data-stu-id="c576a-111">This [documentation provides detailed step-by-step installation instructions](/xamarin/get-started/installation) for each platform.</span></span>

## <a name="download-and-run-the-sample-project"></a><span data-ttu-id="c576a-112">Скачивание и запуск примера проекта</span><span class="sxs-lookup"><span data-stu-id="c576a-112">Download and run the sample project</span></span>

<span data-ttu-id="c576a-113">Чтобы запустить и изучить этот пример приложения, скачайте код на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="c576a-113">To run and explore this sample application, download the code on GitHub.</span></span>

<span data-ttu-id="c576a-114">После скачивания откройте файл решения `EFGettingStarted.sln` в Visual Studio или Visual Studio для Mac и запустите приложение на выбранной платформе.</span><span class="sxs-lookup"><span data-stu-id="c576a-114">Once downloaded, open the solution file `EFGettingStarted.sln` in Visual Studio or Visual Studio for Mac and run the application on the platform of your choice.</span></span>

<span data-ttu-id="c576a-115">При первом запуске приложения локальная база данных SQLite будет заполнена двумя записями, представляющими блоги.</span><span class="sxs-lookup"><span data-stu-id="c576a-115">When the app first starts, it will populate the local SQLite database with two entries representing blogs.</span></span>

![Снимок экрана: страница со списком всех блогов](_static/xamarin-tutorial-1.png)

<span data-ttu-id="c576a-117">На панели инструментов нажмите кнопку **Add** (Добавить).</span><span class="sxs-lookup"><span data-stu-id="c576a-117">Click the **Add** button in the toolbar.</span></span>

<span data-ttu-id="c576a-118">Появится новая страница, на которой можно ввести сведения о новом блоге.</span><span class="sxs-lookup"><span data-stu-id="c576a-118">A new page will appear that allows you to enter information about a new blog.</span></span>

![Снимок экрана: страница изменения сведений о новом блоге](_static/xamarin-tutorial-2.png)

<span data-ttu-id="c576a-120">Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="c576a-120">Fill out all the info and click **Save** from the toolbar.</span></span> <span data-ttu-id="c576a-121">Новый блог сохранится в базе данных SQLite приложения и появится в списке.</span><span class="sxs-lookup"><span data-stu-id="c576a-121">The new blog will save to the app's SQLite database and will show in the list.</span></span>

<span data-ttu-id="c576a-122">Вы можете щелкнуть одну из записей блога в списке и просмотреть все записи в этом блоге.</span><span class="sxs-lookup"><span data-stu-id="c576a-122">You can click on one of the blog entries in the list and see any posts for that blog.</span></span>

![Снимок экрана: страница со списком записей в блоге](_static/xamarin-tutorial-3.png)

<span data-ttu-id="c576a-124">На панели инструментов нажмите кнопку **Add** (Добавить).</span><span class="sxs-lookup"><span data-stu-id="c576a-124">Click **Add** in the toolbar.</span></span>

<span data-ttu-id="c576a-125">Появится страница, на которой можно ввести сведения о новой записи блога.</span><span class="sxs-lookup"><span data-stu-id="c576a-125">A page then appears that allows you to fill out information about a new blog post.</span></span>

![Снимок экрана: страница добавления новой записи](_static/xamarin-tutorial-4.png)

<span data-ttu-id="c576a-127">Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="c576a-127">Fill out all the information and click **Save** in the toolbar.</span></span>

<span data-ttu-id="c576a-128">Новая запись будет связана с записью блога, которую вы выбрали на предыдущем шаге, сохранится в базе данных SQLite приложения и появится в списке.</span><span class="sxs-lookup"><span data-stu-id="c576a-128">The new post will be associated to the blog post you clicked on in a previous step and will save to the app's SQLite database and show in the list.</span></span>

<span data-ttu-id="c576a-129">Вернитесь на страницу со списком блогов.</span><span class="sxs-lookup"><span data-stu-id="c576a-129">Go back to the blog list page.</span></span> <span data-ttu-id="c576a-130">На панели инструментов нажмите кнопку **Delete All** (Удалить все).</span><span class="sxs-lookup"><span data-stu-id="c576a-130">And click **Delete All** in the toolbar.</span></span> <span data-ttu-id="c576a-131">Все блоги и соответствующие записи будут удалены из базы данных SQLite приложения.</span><span class="sxs-lookup"><span data-stu-id="c576a-131">All blogs and their corresponding posts will then be deleted from the app's SQLite database.</span></span>

![Снимок экрана приложения после удаления всех блогов](_static/xamarin-tutorial-5.png)

## <a name="explore-the-code"></a><span data-ttu-id="c576a-133">Обзор кода</span><span class="sxs-lookup"><span data-stu-id="c576a-133">Explore the code</span></span>

<span data-ttu-id="c576a-134">В следующих разделах последовательно разбирается код из образца проекта, который считывает, создает, обновляет и удаляет данные в базе данных SQLite с помощью EF Core с Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="c576a-134">The following sections will walk you through the code in the sample project that reads, creates, updates, and deletes data from a SQLite database using EF Core with Xamarin.Forms.</span></span>

<span data-ttu-id="c576a-135">Предполагается, что вы знаете как [отображать данные](/xamarin/xamarin-forms/app-fundamentals/data-binding/) и [переходить между страницами](/xamarin/xamarin-forms/app-fundamentals/navigation/) с помощью Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="c576a-135">It is assumed that you are familiar with the Xamarin.Forms topics of [displaying data](/xamarin/xamarin-forms/app-fundamentals/data-binding/) and [navigating between pages](/xamarin/xamarin-forms/app-fundamentals/navigation/).</span></span>

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="c576a-136">Пакеты Entity Framework Core NuGet</span><span class="sxs-lookup"><span data-stu-id="c576a-136">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="c576a-137">Чтобы создавать приложения Xamarin.Forms с помощью EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать, во все проекты в решении Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="c576a-137">To create Xamarin.Forms apps with EF Core, you install the package for the EF Core database provider(s) you want to target into all of the projects in the Xamarin.Forms solution.</span></span> <span data-ttu-id="c576a-138">В этом руководстве используется поставщик SQLite.</span><span class="sxs-lookup"><span data-stu-id="c576a-138">This tutorial uses the SQLite provider.</span></span>

<span data-ttu-id="c576a-139">В каждом из проектов решения Xamarin.Forms требуется указанный ниже пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="c576a-139">The following NuGet package is needed in each of the projects in the Xamarin.Forms solution.</span></span>

* `Microsoft.EntityFrameworkCore.Sqlite`

## <a name="model-classes"></a><span data-ttu-id="c576a-140">Классы модели</span><span class="sxs-lookup"><span data-stu-id="c576a-140">Model classes</span></span>

<span data-ttu-id="c576a-141">Каждая таблица в базе данных SQLite, доступ к которой осуществляется через EF Core, моделируется в классе.</span><span class="sxs-lookup"><span data-stu-id="c576a-141">Each table in the SQLite database accessed through EF Core is modeled in a class.</span></span> <span data-ttu-id="c576a-142">В этом примере используются два класса: `Blog` и `Post`. Они находятся в папке `Models`.</span><span class="sxs-lookup"><span data-stu-id="c576a-142">In this sample, two classes are used: `Blog` and `Post` which can be found in the `Models` folder.</span></span>

<span data-ttu-id="c576a-143">Классы модели состоят только из свойств, которые моделируют столбцы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c576a-143">The model classes are composed only of properties, which model columns in the database.</span></span>

* <span data-ttu-id="c576a-144">**Blog.cs**</span><span class="sxs-lookup"><span data-stu-id="c576a-144">**Blog.cs**</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Blog.cs)]

* <span data-ttu-id="c576a-145">Свойство `Posts` определяет связь "родитель-потомок" между `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="c576a-145">The `Posts` property defines a parent-child relationship between `Blog` and `Post`.</span></span>

* <span data-ttu-id="c576a-146">**Post.cs**</span><span class="sxs-lookup"><span data-stu-id="c576a-146">**Post.cs**</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Post.cs)]

* <span data-ttu-id="c576a-147">Свойства `BlogId` и `Blog` определяют обратные связи с родительским объектом `Blog` для экземпляра `Post`.</span><span class="sxs-lookup"><span data-stu-id="c576a-147">The `BlogId` and `Blog` properties relate back to the parent `Blog` object for the instance of the `Post`.</span></span>

## <a name="data-context"></a><span data-ttu-id="c576a-148">Контекст данных</span><span class="sxs-lookup"><span data-stu-id="c576a-148">Data context</span></span>

<span data-ttu-id="c576a-149">Класс `BloggingContext` находится в папке `Services` и наследуется от класса EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c576a-149">The `BloggingContext` class is located in the `Services` folder and inherits from the EF Core `DbContext` class.</span></span> <span data-ttu-id="c576a-150">`DbContext` используется для группирования запросов к базе данных и изменений.</span><span class="sxs-lookup"><span data-stu-id="c576a-150">A `DbContext` is used to group together database queries and changes.</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Services/BloggingContext.cs)]

* <span data-ttu-id="c576a-151">Оба свойства в этом классе типа `DbSet` служат для работы с базовыми таблицами, представляющими блоги и записи.</span><span class="sxs-lookup"><span data-stu-id="c576a-151">Both properties in this class of type `DbSet` are used to operate on the underlying tables representing Blogs and Posts.</span></span>
* <span data-ttu-id="c576a-152">`SQLitePCL.Batteries_V2.Init()` требуется в конструкторе для инициации SQLite в iOS.</span><span class="sxs-lookup"><span data-stu-id="c576a-152">The `SQLitePCL.Batteries_V2.Init()` is needed in the constructor to initiate SQLite on iOS.</span></span>
* <span data-ttu-id="c576a-153">Функция `OnConfiguring` задает расположение базы данных SQLite на физическом устройстве.</span><span class="sxs-lookup"><span data-stu-id="c576a-153">The `OnConfiguring` function sets up the location of the SQLite database on the physical device.</span></span>

## <a name="create-read-update--delete"></a><span data-ttu-id="c576a-154">Создание, чтение, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="c576a-154">Create, read, update & delete</span></span>

<span data-ttu-id="c576a-155">Ниже приведены некоторые места в приложении, где EF Core используется для доступа к SQLite.</span><span class="sxs-lookup"><span data-stu-id="c576a-155">The following are some instances in the app where EF Core is used to access SQLite.</span></span>

### <a name="read"></a><span data-ttu-id="c576a-156">Чтение</span><span class="sxs-lookup"><span data-stu-id="c576a-156">Read</span></span>

* <span data-ttu-id="c576a-157">Возврат всех записей.</span><span class="sxs-lookup"><span data-stu-id="c576a-157">Return all records.</span></span>
  * <span data-ttu-id="c576a-158">Функция `OnAppearing` в `BlogsPage.xaml.cs` возвращает все записи `Blog` и сохраняет их в переменной `List`.</span><span class="sxs-lookup"><span data-stu-id="c576a-158">The `OnAppearing` function of `BlogsPage.xaml.cs` returns all `Blog` records and stores them into a `List` variable.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    var theBlogs = blogContext.Blogs.ToList();
}
```

* <span data-ttu-id="c576a-159">Возврат определенных записей.</span><span class="sxs-lookup"><span data-stu-id="c576a-159">Return specific records.</span></span>
  * <span data-ttu-id="c576a-160">Функция `OnAppearing` в `PostsPage.xaml.cs` возвращает записи `Post`, содержащие определенное значение `BlogId`.</span><span class="sxs-lookup"><span data-stu-id="c576a-160">The `OnAppearing` function of `PostsPage.xaml.cs` returns `Post` records that contain a specific `BlogId`.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    var postList = blogContext.Posts
        .Where(p => p.BlogId == BlogId)
        .ToList();
}
```

### <a name="create"></a><span data-ttu-id="c576a-161">Создать</span><span class="sxs-lookup"><span data-stu-id="c576a-161">Create</span></span>

* <span data-ttu-id="c576a-162">Вставка новой записи.</span><span class="sxs-lookup"><span data-stu-id="c576a-162">Insert a new record.</span></span>
  * <span data-ttu-id="c576a-163">Функция `Save_Clicked` в `AddBlogPage.xaml.cs` вставляет новый объект `Blog` в базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="c576a-163">The `Save_Clicked` function of `AddBlogPage.xaml.cs` inserts a new `Blog` object into the SQLite database.</span></span>

```csharp
var blog = new Blog { Url = blogUrl.Text };

using (var blogContext = new BloggingContext())
{
    blogContext.Add(blog);

    await blogContext.SaveChangesAsync();
}
```

### <a name="update"></a><span data-ttu-id="c576a-164">Обновление</span><span class="sxs-lookup"><span data-stu-id="c576a-164">Update</span></span>

* <span data-ttu-id="c576a-165">Изменение существующей записи.</span><span class="sxs-lookup"><span data-stu-id="c576a-165">Update an existing record.</span></span>
  * <span data-ttu-id="c576a-166">Функция `Save_Clicked` в `AddPostPage.xaml.cs` изменяет существующий объект `Blog`, используя новый объект `Post`.</span><span class="sxs-lookup"><span data-stu-id="c576a-166">The `Save_Clicked` function of `AddPostPage.xaml.cs` updates an existing `Blog` object with a new `Post`.</span></span>
  
```csharp
var newPost = new Post
{
    BlogId = BlogId,
    Content = postCell.Text,
    Title = titleCell.Text
};

using (var blogContext = new BloggingContext())
{
    var blog = await blogContext
        .Blogs
        .FirstAsync(b => b.BlogId == BlogId);

    blog.Posts.Add(newPost);

    await blogContext.SaveChangesAsync();
}
```

### <a name="delete"></a><span data-ttu-id="c576a-167">Удаление</span><span class="sxs-lookup"><span data-stu-id="c576a-167">Delete</span></span>

* <span data-ttu-id="c576a-168">Удаление всех записей, включая дочерние.</span><span class="sxs-lookup"><span data-stu-id="c576a-168">Delete all records with cascade to child records.</span></span>
  * <span data-ttu-id="c576a-169">Функция `DeleteAll_Clicked` в `BlogsPage.xaml.cs` удаляет все записи `Blog` в базе данных SQLite, распространяя удаление на все дочерние записи `Post` объекта `Blog`.</span><span class="sxs-lookup"><span data-stu-id="c576a-169">The `DeleteAll_Clicked` function of `BlogsPage.xaml.cs` deletes all the `Blog` records in the SQLite database and cascades the deletes to all of the `Blog` child `Post` records.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    blogContext.RemoveRange(blogContext.Blogs);

    await blogContext.SaveChangesAsync();
}
```

## <a name="next-steps"></a><span data-ttu-id="c576a-170">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c576a-170">Next steps</span></span>

<span data-ttu-id="c576a-171">В этом кратком руководстве вы узнали, как использовать приложение Xamarin.Forms для доступа к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c576a-171">In this getting started you have learned how to use a Xamarin.Forms application to access a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="c576a-172">Другие разделы по Entity Framework Core, которые могут представлять интерес для разработчиков Xamarin:</span><span class="sxs-lookup"><span data-stu-id="c576a-172">Other Entity Framework Core topics of interest to Xamarin developers:</span></span>

* [<span data-ttu-id="c576a-173">Настройка `DbContext`</span><span class="sxs-lookup"><span data-stu-id="c576a-173">Configuring a `DbContext`</span></span>](xref:core/miscellaneous/configuring-dbcontext)
* <span data-ttu-id="c576a-174">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="c576a-174">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="c576a-175">[Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="c576a-175">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
