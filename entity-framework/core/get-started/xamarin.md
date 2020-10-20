---
title: Начало работы с EF Core и Xamarin — EF Core
description: Руководство по началу работы с Xamarin для создания мобильных приложений с помощью Entity Framework Core
author: codemillmatt
ms.date: 07/07/2020
ms.author: masoucou
uid: core/get-started/xamarin
ms.openlocfilehash: 0552038d471e294834bed9e3bf1f05fd74c39192
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062546"
---
# <a name="getting-started-with-ef-core-and-xamarin"></a><span data-ttu-id="8b083-103">Начало работы с EF Core и Xamarin</span><span class="sxs-lookup"><span data-stu-id="8b083-103">Getting Started with EF Core and Xamarin</span></span>

<span data-ttu-id="8b083-104">В этом руководстве вы создадите приложение [Xamarin.Forms](/xamarin/get-started/what-is-xamarin-forms), которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8b083-104">In this tutorial, you create a [Xamarin.Forms](/xamarin/get-started/what-is-xamarin-forms) application that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="8b083-105">Инструкции из этого руководства можно выполнять с помощью Visual Studio в Windows или Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="8b083-105">You can follow the tutorial by using Visual Studio on Windows or Visual Studio for Mac.</span></span>

> [!TIP]
> <span data-ttu-id="8b083-106">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Xamarin) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="8b083-106">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Xamarin).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b083-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8b083-107">Prerequisites</span></span>

<span data-ttu-id="8b083-108">Установите одно из следующих приложений:</span><span class="sxs-lookup"><span data-stu-id="8b083-108">Install one of the below:</span></span>

* <span data-ttu-id="8b083-109">[Visual Studio 2019 версии 16.3 или более поздней](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:</span><span class="sxs-lookup"><span data-stu-id="8b083-109">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this workload:</span></span>
  * <span data-ttu-id="8b083-110">**Разработка мобильных приложений на .NET**</span><span class="sxs-lookup"><span data-stu-id="8b083-110">**Mobile Development with .NET**</span></span>
* [<span data-ttu-id="8b083-111">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="8b083-111">Visual Studio for Mac</span></span>](https://visualstudio.microsoft.com/vs/mac/)

<span data-ttu-id="8b083-112">В этой [документации содержатся подробные пошаговые инструкции по установке](/xamarin/get-started/installation) для каждой платформы.</span><span class="sxs-lookup"><span data-stu-id="8b083-112">This [documentation provides detailed step-by-step installation instructions](/xamarin/get-started/installation) for each platform.</span></span>

## <a name="download-and-run-the-sample-project"></a><span data-ttu-id="8b083-113">Скачивание и запуск примера проекта</span><span class="sxs-lookup"><span data-stu-id="8b083-113">Download and run the sample project</span></span>

<span data-ttu-id="8b083-114">Чтобы запустить и изучить этот пример приложения, скачайте код на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="8b083-114">To run and explore this sample application, download the code on GitHub.</span></span>

<span data-ttu-id="8b083-115">После скачивания откройте файл решения `EFGettingStarted.sln` в Visual Studio или Visual Studio для Mac и запустите приложение на выбранной платформе.</span><span class="sxs-lookup"><span data-stu-id="8b083-115">Once downloaded, open the solution file `EFGettingStarted.sln` in Visual Studio or Visual Studio for Mac and run the application on the platform of your choice.</span></span>

<span data-ttu-id="8b083-116">При первом запуске приложения локальная база данных SQLite будет заполнена двумя записями, представляющими блоги.</span><span class="sxs-lookup"><span data-stu-id="8b083-116">When the app first starts, it will populate the local SQLite database with two entries representing blogs.</span></span>

![Снимок экрана: страница со списком всех блогов](_static/xamarin-tutorial-1.png)

<span data-ttu-id="8b083-118">На панели инструментов нажмите кнопку **Add** (Добавить).</span><span class="sxs-lookup"><span data-stu-id="8b083-118">Click the **Add** button in the toolbar.</span></span>

<span data-ttu-id="8b083-119">Появится новая страница, на которой можно ввести сведения о новом блоге.</span><span class="sxs-lookup"><span data-stu-id="8b083-119">A new page will appear that allows you to enter information about a new blog.</span></span>

![Снимок экрана: страница изменения сведений о новом блоге](_static/xamarin-tutorial-2.png)

<span data-ttu-id="8b083-121">Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="8b083-121">Fill out all the info and click **Save** from the toolbar.</span></span> <span data-ttu-id="8b083-122">Новый блог сохранится в базе данных SQLite приложения и появится в списке.</span><span class="sxs-lookup"><span data-stu-id="8b083-122">The new blog will save to the app's SQLite database and will show in the list.</span></span>

<span data-ttu-id="8b083-123">Вы можете щелкнуть одну из записей блога в списке и просмотреть все записи в этом блоге.</span><span class="sxs-lookup"><span data-stu-id="8b083-123">You can click on one of the blog entries in the list and see any posts for that blog.</span></span>

![Снимок экрана: страница со списком записей в блоге](_static/xamarin-tutorial-3.png)

<span data-ttu-id="8b083-125">На панели инструментов нажмите кнопку **Add** (Добавить).</span><span class="sxs-lookup"><span data-stu-id="8b083-125">Click **Add** in the toolbar.</span></span>

<span data-ttu-id="8b083-126">Появится страница, на которой можно ввести сведения о новой записи блога.</span><span class="sxs-lookup"><span data-stu-id="8b083-126">A page then appears that allows you to fill out information about a new blog post.</span></span>

![Снимок экрана: страница добавления новой записи](_static/xamarin-tutorial-4.png)

<span data-ttu-id="8b083-128">Введите все сведения и на панели инструментов нажмите кнопку **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="8b083-128">Fill out all the information and click **Save** in the toolbar.</span></span>

<span data-ttu-id="8b083-129">Новая запись будет связана с записью блога, которую вы выбрали на предыдущем шаге, сохранится в базе данных SQLite приложения и появится в списке.</span><span class="sxs-lookup"><span data-stu-id="8b083-129">The new post will be associated to the blog post you clicked on in a previous step and will save to the app's SQLite database and show in the list.</span></span>

<span data-ttu-id="8b083-130">Вернитесь на страницу со списком блогов.</span><span class="sxs-lookup"><span data-stu-id="8b083-130">Go back to the blog list page.</span></span> <span data-ttu-id="8b083-131">На панели инструментов нажмите кнопку **Delete All** (Удалить все).</span><span class="sxs-lookup"><span data-stu-id="8b083-131">And click **Delete All** in the toolbar.</span></span> <span data-ttu-id="8b083-132">Все блоги и соответствующие записи будут удалены из базы данных SQLite приложения.</span><span class="sxs-lookup"><span data-stu-id="8b083-132">All blogs and their corresponding posts will then be deleted from the app's SQLite database.</span></span>

![Снимок экрана приложения после удаления всех блогов](_static/xamarin-tutorial-5.png)

## <a name="explore-the-code"></a><span data-ttu-id="8b083-134">Обзор кода</span><span class="sxs-lookup"><span data-stu-id="8b083-134">Explore the code</span></span>

<span data-ttu-id="8b083-135">В следующих разделах последовательно разбирается код из образца проекта, который считывает, создает, обновляет и удаляет данные в базе данных SQLite с помощью EF Core с Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="8b083-135">The following sections will walk you through the code in the sample project that reads, creates, updates, and deletes data from a SQLite database using EF Core with Xamarin.Forms.</span></span>

<span data-ttu-id="8b083-136">Предполагается, что вы знаете как [отображать данные](/xamarin/xamarin-forms/app-fundamentals/data-binding/) и [переходить между страницами](/xamarin/xamarin-forms/app-fundamentals/navigation/) с помощью Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="8b083-136">It is assumed that you are familiar with the Xamarin.Forms topics of [displaying data](/xamarin/xamarin-forms/app-fundamentals/data-binding/) and [navigating between pages](/xamarin/xamarin-forms/app-fundamentals/navigation/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8b083-137">Entity Framework Core применяет отражение для вызова функций, которые компоновщик Xamarin.iOS может исключить, если используются конфигурации режима **Выпуск**.</span><span class="sxs-lookup"><span data-stu-id="8b083-137">Entity Framework Core uses reflection to invoke functions which the Xamarin.iOS linker may strip out while in **Release** mode configurations.</span></span> <span data-ttu-id="8b083-138">Предотвратить это можно одним из двух способов.</span><span class="sxs-lookup"><span data-stu-id="8b083-138">You can avoid that in one of two ways.</span></span>
> 
> * <span data-ttu-id="8b083-139">Первый способ: добавьте `--linkskip System.Core` в поле **Дополнительные аргументы mtouch** в параметрах **сборки iOS**.</span><span class="sxs-lookup"><span data-stu-id="8b083-139">The first is to add `--linkskip System.Core` to the **Additional mtouch arguments** in the **iOS Build** options.</span></span>
> * <span data-ttu-id="8b083-140">Второй способ: в параметрах **сборки iOS** задайте для параметра **Поведение компоновщика** Xamarin.iOS значение `Don't Link`.</span><span class="sxs-lookup"><span data-stu-id="8b083-140">Alternatively set the Xamarin.iOS **Linker behavior** to `Don't Link` in the **iOS Build** options.</span></span>
> <span data-ttu-id="8b083-141">Дополнительные сведения о компоновщике Xamarin.iOS, включая настройку его поведения для Xamarin.iOS, приведены в [этой статье](/xamarin/ios/deploy-test/linker).</span><span class="sxs-lookup"><span data-stu-id="8b083-141">[This article explains more about the Xamarin.iOS linker](/xamarin/ios/deploy-test/linker) including how to set the behavior on Xamarin.iOS.</span></span>
> 

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="8b083-142">Пакеты Entity Framework Core NuGet</span><span class="sxs-lookup"><span data-stu-id="8b083-142">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="8b083-143">Чтобы создавать приложения Xamarin.Forms с помощью EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать, во все проекты в решении Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="8b083-143">To create Xamarin.Forms apps with EF Core, you install the package for the EF Core database provider(s) you want to target into all of the projects in the Xamarin.Forms solution.</span></span> <span data-ttu-id="8b083-144">В этом руководстве используется поставщик SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b083-144">This tutorial uses the SQLite provider.</span></span>

<span data-ttu-id="8b083-145">В каждом из проектов решения Xamarin.Forms требуется указанный ниже пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b083-145">The following NuGet package is needed in each of the projects in the Xamarin.Forms solution.</span></span>

* `Microsoft.EntityFrameworkCore.Sqlite`

## <a name="model-classes"></a><span data-ttu-id="8b083-146">Классы модели</span><span class="sxs-lookup"><span data-stu-id="8b083-146">Model classes</span></span>

<span data-ttu-id="8b083-147">Каждая таблица в базе данных SQLite, доступ к которой осуществляется через EF Core, моделируется в классе.</span><span class="sxs-lookup"><span data-stu-id="8b083-147">Each table in the SQLite database accessed through EF Core is modeled in a class.</span></span> <span data-ttu-id="8b083-148">В этом примере используются два класса: `Blog` и `Post`. Они находятся в папке `Models`.</span><span class="sxs-lookup"><span data-stu-id="8b083-148">In this sample, two classes are used: `Blog` and `Post` which can be found in the `Models` folder.</span></span>

<span data-ttu-id="8b083-149">Классы модели состоят только из свойств, которые моделируют столбцы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8b083-149">The model classes are composed only of properties, which model columns in the database.</span></span>

* <span data-ttu-id="8b083-150">**Blog.cs**</span><span class="sxs-lookup"><span data-stu-id="8b083-150">**Blog.cs**</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Blog.cs)]

* <span data-ttu-id="8b083-151">Свойство `Posts` определяет связь "родитель-потомок" между `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="8b083-151">The `Posts` property defines a parent-child relationship between `Blog` and `Post`.</span></span>

* <span data-ttu-id="8b083-152">**Post.cs**</span><span class="sxs-lookup"><span data-stu-id="8b083-152">**Post.cs**</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Models/Post.cs)]

* <span data-ttu-id="8b083-153">Свойства `BlogId` и `Blog` определяют обратные связи с родительским объектом `Blog` для экземпляра `Post`.</span><span class="sxs-lookup"><span data-stu-id="8b083-153">The `BlogId` and `Blog` properties relate back to the parent `Blog` object for the instance of the `Post`.</span></span>

## <a name="data-context"></a><span data-ttu-id="8b083-154">Контекст данных</span><span class="sxs-lookup"><span data-stu-id="8b083-154">Data context</span></span>

<span data-ttu-id="8b083-155">Класс `BloggingContext` находится в папке `Services` и наследуется от класса EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8b083-155">The `BloggingContext` class is located in the `Services` folder and inherits from the EF Core `DbContext` class.</span></span> <span data-ttu-id="8b083-156">`DbContext` используется для группирования запросов к базе данных и изменений.</span><span class="sxs-lookup"><span data-stu-id="8b083-156">A `DbContext` is used to group together database queries and changes.</span></span>

  [!code-csharp[](../../../samples/core/Xamarin/EFGetStarted/Services/BloggingContext.cs)]

* <span data-ttu-id="8b083-157">Оба свойства в этом классе типа `DbSet` служат для работы с базовыми таблицами, представляющими блоги и записи.</span><span class="sxs-lookup"><span data-stu-id="8b083-157">Both properties in this class of type `DbSet` are used to operate on the underlying tables representing Blogs and Posts.</span></span>
* <span data-ttu-id="8b083-158">`SQLitePCL.Batteries_V2.Init()` требуется в конструкторе для инициации SQLite в iOS.</span><span class="sxs-lookup"><span data-stu-id="8b083-158">The `SQLitePCL.Batteries_V2.Init()` is needed in the constructor to initiate SQLite on iOS.</span></span>
* <span data-ttu-id="8b083-159">Функция `OnConfiguring` задает расположение базы данных SQLite на физическом устройстве.</span><span class="sxs-lookup"><span data-stu-id="8b083-159">The `OnConfiguring` function sets up the location of the SQLite database on the physical device.</span></span>

## <a name="create-read-update--delete"></a><span data-ttu-id="8b083-160">Создание, чтение, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="8b083-160">Create, read, update & delete</span></span>

<span data-ttu-id="8b083-161">Ниже приведены некоторые места в приложении, где EF Core используется для доступа к SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b083-161">The following are some instances in the app where EF Core is used to access SQLite.</span></span>

### <a name="read"></a><span data-ttu-id="8b083-162">Чтение</span><span class="sxs-lookup"><span data-stu-id="8b083-162">Read</span></span>

* <span data-ttu-id="8b083-163">Возврат всех записей.</span><span class="sxs-lookup"><span data-stu-id="8b083-163">Return all records.</span></span>
  * <span data-ttu-id="8b083-164">Функция `OnAppearing` в `BlogsPage.xaml.cs` возвращает все записи `Blog` и сохраняет их в переменной `List`.</span><span class="sxs-lookup"><span data-stu-id="8b083-164">The `OnAppearing` function of `BlogsPage.xaml.cs` returns all `Blog` records and stores them into a `List` variable.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    var theBlogs = blogContext.Blogs.ToList();
}
```

* <span data-ttu-id="8b083-165">Возврат определенных записей.</span><span class="sxs-lookup"><span data-stu-id="8b083-165">Return specific records.</span></span>
  * <span data-ttu-id="8b083-166">Функция `OnAppearing` в `PostsPage.xaml.cs` возвращает записи `Post`, содержащие определенное значение `BlogId`.</span><span class="sxs-lookup"><span data-stu-id="8b083-166">The `OnAppearing` function of `PostsPage.xaml.cs` returns `Post` records that contain a specific `BlogId`.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    var postList = blogContext.Posts
        .Where(p => p.BlogId == BlogId)
        .ToList();
}
```

### <a name="create"></a><span data-ttu-id="8b083-167">Создать</span><span class="sxs-lookup"><span data-stu-id="8b083-167">Create</span></span>

* <span data-ttu-id="8b083-168">Вставка новой записи.</span><span class="sxs-lookup"><span data-stu-id="8b083-168">Insert a new record.</span></span>
  * <span data-ttu-id="8b083-169">Функция `Save_Clicked` в `AddBlogPage.xaml.cs` вставляет новый объект `Blog` в базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b083-169">The `Save_Clicked` function of `AddBlogPage.xaml.cs` inserts a new `Blog` object into the SQLite database.</span></span>

```csharp
var blog = new Blog { Url = blogUrl.Text };

using (var blogContext = new BloggingContext())
{
    blogContext.Add(blog);

    await blogContext.SaveChangesAsync();
}
```

### <a name="update"></a><span data-ttu-id="8b083-170">Обновление</span><span class="sxs-lookup"><span data-stu-id="8b083-170">Update</span></span>

* <span data-ttu-id="8b083-171">Изменение существующей записи.</span><span class="sxs-lookup"><span data-stu-id="8b083-171">Update an existing record.</span></span>
  * <span data-ttu-id="8b083-172">Функция `Save_Clicked` в `AddPostPage.xaml.cs` изменяет существующий объект `Blog`, используя новый объект `Post`.</span><span class="sxs-lookup"><span data-stu-id="8b083-172">The `Save_Clicked` function of `AddPostPage.xaml.cs` updates an existing `Blog` object with a new `Post`.</span></span>
  
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

### <a name="delete"></a><span data-ttu-id="8b083-173">Удаление</span><span class="sxs-lookup"><span data-stu-id="8b083-173">Delete</span></span>

* <span data-ttu-id="8b083-174">Удаление всех записей, включая дочерние.</span><span class="sxs-lookup"><span data-stu-id="8b083-174">Delete all records with cascade to child records.</span></span>
  * <span data-ttu-id="8b083-175">Функция `DeleteAll_Clicked` в `BlogsPage.xaml.cs` удаляет все записи `Blog` в базе данных SQLite, распространяя удаление на все дочерние записи `Post` объекта `Blog`.</span><span class="sxs-lookup"><span data-stu-id="8b083-175">The `DeleteAll_Clicked` function of `BlogsPage.xaml.cs` deletes all the `Blog` records in the SQLite database and cascades the deletes to all of the `Blog` child `Post` records.</span></span>

```csharp
using (var blogContext = new BloggingContext())
{
    blogContext.RemoveRange(blogContext.Blogs);

    await blogContext.SaveChangesAsync();
}
```

## <a name="next-steps"></a><span data-ttu-id="8b083-176">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8b083-176">Next steps</span></span>

<span data-ttu-id="8b083-177">В этом кратком руководстве вы узнали, как использовать приложение Xamarin.Forms для доступа к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8b083-177">In this getting started you have learned how to use a Xamarin.Forms application to access a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="8b083-178">Другие разделы по Entity Framework Core, которые могут представлять интерес для разработчиков Xamarin:</span><span class="sxs-lookup"><span data-stu-id="8b083-178">Other Entity Framework Core topics of interest to Xamarin developers:</span></span>

* [<span data-ttu-id="8b083-179">Настройка `DbContext`</span><span class="sxs-lookup"><span data-stu-id="8b083-179">Configuring a `DbContext`</span></span>](xref:core/miscellaneous/configuring-dbcontext)
* <span data-ttu-id="8b083-180">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="8b083-180">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="8b083-181">[Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="8b083-181">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
