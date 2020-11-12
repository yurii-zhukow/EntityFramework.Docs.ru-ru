---
title: Начало работы с WPF — EF Core
description: Руководство по началу работы с WPF с использованием Entity Framework Core
author: jeremylikness
ms.author: jeliknes
ms.date: 07/24/2020
uid: core/get-started/wpf
ms.openlocfilehash: f183064fafbe2d0e7b8dbdafa921169afc9ffe78
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429927"
---
# <a name="getting-started-with-wpf"></a><span data-ttu-id="a4ec9-103">Начало работы с WPF</span><span class="sxs-lookup"><span data-stu-id="a4ec9-103">Getting Started with WPF</span></span>

<span data-ttu-id="a4ec9-104">В этом пошаговом руководстве показано, как привязать типы POCO к элементам управления WPF в форме "Основное — сведения".</span><span class="sxs-lookup"><span data-stu-id="a4ec9-104">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a "main-detail" form.</span></span> <span data-ttu-id="a4ec9-105">Приложение использует API-интерфейсы Entity Framework для заполнения объектов данными из базы данных, трассировки изменений и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-105">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="a4ec9-106">Модель определяет два типа, которые участвуют в связи "один ко многим": **Category** (principal\\main) и **Product** (dependent\\detail).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-106">The model defines two types that participate in one-to-many relationship: **Category** (principal\\main) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="a4ec9-107">Платформа привязки данных WPF позволяет переходить между связанными объектами: выбор строки в главном представлении приводит к обновлению подробного представления соответствующими данными дочернего объекта.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="a4ec9-108">Снимки экрана и листинги кода в этом пошаговом руководстве взяты из Visual Studio 2019 16.6.5.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2019 16.6.5.</span></span>

> [!TIP]
> <span data-ttu-id="a4ec9-109">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/WPF) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-109">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/WPF).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a4ec9-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a4ec9-110">Pre-Requisites</span></span>

<span data-ttu-id="a4ec9-111">Для выполнения этого пошагового руководства необходимо установить Visual Studio 2019 16.3 или более поздней версии и выбрать при этом **рабочую нагрузку для классических приложений .NET**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-111">You need to have Visual Studio 2019 16.3 or later installed with the **.NET desktop workload** selected to complete this walkthrough.</span></span> <span data-ttu-id="a4ec9-112">Дополнительные сведения об установке новейшей версии Visual Studio см. в статье [Установка Visual Studio](/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-112">For more information about installing the latest version of Visual Studio, see [Install Visual Studio](/visualstudio/install/install-visual-studio).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="a4ec9-113">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="a4ec9-113">Create the Application</span></span>

1. <span data-ttu-id="a4ec9-114">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4ec9-114">Open Visual Studio</span></span>
2. <span data-ttu-id="a4ec9-115">На начальном экране выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-115">On the start window, choose **Create new project**.</span></span>
3. <span data-ttu-id="a4ec9-116">Выполните поиск по запросу "WPF", выберите в результатах **Приложение WPF (.NET Core)** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-116">Search for "WPF," choose **WPF App (.NET Core)** and then choose **Next**.</span></span>
4. <span data-ttu-id="a4ec9-117">На следующем экране присвойте проекту имя, например **GetStartedWPF** , и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-117">At the next screen, give the project a name, for example, **GetStartedWPF** , and choose **Create.**</span></span>

## <a name="install-the-entity-framework-nuget-packages"></a><span data-ttu-id="a4ec9-118">Установка пакета NuGet для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a4ec9-118">Install the Entity Framework NuGet packages</span></span>

1. <span data-ttu-id="a4ec9-119">Щелкните правой кнопкой мыши решение и выберите **Управление пакетами NuGet для решения...**</span><span class="sxs-lookup"><span data-stu-id="a4ec9-119">Right-click on the solution and choose **Manage NuGet Packages for Solution...**</span></span>

    ![Управление пакетами NuGet](_static/wpf-tutorial-nuget.jpg)

1. <span data-ttu-id="a4ec9-121">Введите `entityframeworkcore.sqlite` в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-121">Type `entityframeworkcore.sqlite` in the search box.</span></span>
1. <span data-ttu-id="a4ec9-122">Выберите пакет **Microsoft.EntityFrameworkCore.Sqlite**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-122">Select the **Microsoft.EntityFrameworkCore.Sqlite** package.</span></span>
1. <span data-ttu-id="a4ec9-123">Установите флажок рядом с проектом в области справа и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-123">Check the project in the right pane and click **Install**</span></span>

    ![Пакет SQLite](_static/wpf-tutorial-sqlite.jpg)

1. <span data-ttu-id="a4ec9-125">Повторите шаги для поиска `entityframeworkcore.proxies` и установите **Microsoft.EntityFrameworkCore.Proxies**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-125">Repeat the steps to search for `entityframeworkcore.proxies` and install **Microsoft.EntityFrameworkCore.Proxies**.</span></span>

> [!NOTE]
> <span data-ttu-id="a4ec9-126">При установке пакета SQLite автоматически извлекается связанный базовый пакет **Microsoft.EntityFrameworkCore**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-126">When you installed the Sqlite package, it automatically pulled down the related **Microsoft.EntityFrameworkCore** base package.</span></span> <span data-ttu-id="a4ec9-127">Пакет **Microsoft.EntityFrameworkCore.Proxies** обеспечивает поддержку "отложенной загрузки" данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-127">The **Microsoft.EntityFrameworkCore.Proxies** package provides support for "lazy-loading" data.</span></span> <span data-ttu-id="a4ec9-128">Это означает, что при наличии сущностей с дочерними объектами в процессе начальной нагрузки извлекаются только родительские сущности.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-128">This means when you have entities with child entities, only the parents are fetched on the initial load.</span></span> <span data-ttu-id="a4ec9-129">Прокси обнаруживают попытку доступа к дочерним сущностям и автоматически загружает их по требованию.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-129">The proxies detect when an attempt to access the child entities is made and automatically loads them on demand.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="a4ec9-130">Определение модели</span><span class="sxs-lookup"><span data-stu-id="a4ec9-130">Define a Model</span></span>

<span data-ttu-id="a4ec9-131">В этом пошаговом руководстве описана реализация модели с использованием Code First.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-131">In this walkthrough you will implement a model using "code first."</span></span> <span data-ttu-id="a4ec9-132">Таким образом, EF Core создаст таблицы и схему базы данных на основе определенных вами классов C#.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-132">This means that EF Core will create the database tables and schema based on the C# classes you define.</span></span>

<span data-ttu-id="a4ec9-133">Добавьте новый класс.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-133">Add a new class.</span></span> <span data-ttu-id="a4ec9-134">Присвойте ему имя `Product.cs` и заполните его следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a4ec9-134">Give it the name: `Product.cs` and populate it like this:</span></span>

**`Product.cs`**

[!code-csharp[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/Product.cs)]

<span data-ttu-id="a4ec9-135">Затем добавьте класс с именем `Category.cs` и заполните его следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a4ec9-135">Next, add a class named `Category.cs` and populate it with the following code:</span></span>

**`Category.cs`**

[!code-csharp[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/Category.cs)]

<span data-ttu-id="a4ec9-136">Свойство **Products** класса **Category** и свойство **Category** класса **Product**  — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-136">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="a4ec9-137">В Entity Framework свойства навигации обеспечивают возможность навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-137">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="a4ec9-138">В дополнение к определению сущностей необходимо определить класс, производный от DbContext и предоставляющий свойства DbSet&lt;TEntity&gt;.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-138">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="a4ec9-139">Свойства DbSet&lt;TEntity&gt; позволяют контексту определить, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-139">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="a4ec9-140">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, в частности заполняет объекты данными из базы данных, отслеживает изменения и сохраняет данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-140">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="a4ec9-141">Добавьте в проект новый класс `ProductContext.cs` со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="a4ec9-141">Add a new `ProductContext.cs` class to the project with the following definition:</span></span>

**`ProductContext.cs`**

[!code-csharp[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/ProductContext.cs)]

* <span data-ttu-id="a4ec9-142">`DbSet` сообщает EF Core, какие сущности C# следует сопоставить с базой данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-142">The `DbSet` informs EF Core what C# entities should be mapped to the database.</span></span>
* <span data-ttu-id="a4ec9-143">`DbContext` для EF Core можно настроить множеством способов.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-143">There are a variety of ways to configure the EF Core `DbContext`.</span></span> <span data-ttu-id="a4ec9-144">Информация о них приведена в статье [Настройка DbContext](xref:core/dbcontext-configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-144">You can read about them in: [Configuring a DbContext](xref:core/dbcontext-configuration/index).</span></span>
* <span data-ttu-id="a4ec9-145">Чтобы задать файл данных SQLite, в этом примере используется переопределение `OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-145">This example uses the `OnConfiguring` override to specify a Sqlite data file.</span></span>
* <span data-ttu-id="a4ec9-146">Вызов `UseLazyLoadingProxies` указывает EF Core, что необходимо реализовать отложенную загрузку, благодаря которой дочерние сущности автоматически загружаются при доступе из родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-146">The `UseLazyLoadingProxies` call tells EF Core to implement lazy-loading, so child entities are automatically loaded when accessed from the parent.</span></span>

<span data-ttu-id="a4ec9-147">Нажмите клавиши **CTRL+SHIFT+B** или последовательно выберите **Сборка&gt; Собрать решение** для компиляции проекта.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-147">Press **CTRL+SHIFT+B** or navigate to **Build &gt; Build Solution** to compile the project.</span></span>

> [!TIP]
> <span data-ttu-id="a4ec9-148">Сведения о различных способах синхронизации базы данных и моделей EF Core см. в статье [Управление схемами баз данных](xref:core/managing-schemas/index).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-148">Learn about the different was to keep your database and EF Core models in sync: [Managing Database Schemas](xref:core/managing-schemas/index).</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="a4ec9-149">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="a4ec9-149">Lazy Loading</span></span>

<span data-ttu-id="a4ec9-150">Свойство **Products** класса **Category** и свойство **Category** класса **Product**  — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-150">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="a4ec9-151">В Entity Framework Core свойства навигации обеспечивают возможность навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-151">In Entity Framework Core, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="a4ec9-152">EF Core обеспечивает возможность автоматической загрузки связанных сущностей из базы данных при первом доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-152">EF Core gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="a4ec9-153">В случае использования такого типа загрузки (называемой отложенной) следует помнить о следующем: если содержимое еще отсутствует в контексте, при первом обращении к каждому свойству навигации будет выполняться отдельный запрос базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-153">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="a4ec9-154">При использовании типов сущностей POCO решение EF Core обеспечивает отложенную загрузку, создавая во время выполнения экземпляры производных прокси-типов, а затем переопределяя виртуальные свойства в классах для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-154">When using "Plain Old C# Object" (POCO) entity types, EF Core achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="a4ec9-155">Чтобы получить отложенную загрузку связанных объектов, необходимо объявить методы получения свойств навигации как **общедоступные** и **виртуальные** ( **переопределяемые** в Visual Basic). При этом класс не должен быть **запечатанным** ( **непереопределяемым** в Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-155">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** ( **Overridable** in Visual Basic), and your class must not be **sealed** ( **NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="a4ec9-156">При использовании Database First свойства навигации автоматически становятся виртуальными для включения возможности отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-156">When using Database First, navigation properties are automatically made virtual to enable lazy loading.</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="a4ec9-157">Привязка объекта к элементам управления</span><span class="sxs-lookup"><span data-stu-id="a4ec9-157">Bind Object to Controls</span></span>

<span data-ttu-id="a4ec9-158">Добавьте классы, определенные в модели в качестве источников данных для этого приложения WPF.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-158">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

1. <span data-ttu-id="a4ec9-159">Дважды щелкните файл **MainWindow.xaml** в Обозревателе решений, чтобы открыть главную форму.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-159">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
1. <span data-ttu-id="a4ec9-160">Перейдите на вкладку **XAML** , чтобы изменить код XAML.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-160">Choose the **XAML** tab to edit the XAML.</span></span>
1. <span data-ttu-id="a4ec9-161">Сразу после открывающего тега `Window` добавьте следующие источники для подключения к сущностям EF Core.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-161">Immediately after the opening `Window` tag, add the following sources to connect to the EF Core entities.</span></span>

    [!code-xaml[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/MainWindow.xaml?range=1-13&highlight=9-13)]

1. <span data-ttu-id="a4ec9-162">Таким образом будет задан один источник для "родительских" категорий, а второй — для продуктов "сведения".</span><span class="sxs-lookup"><span data-stu-id="a4ec9-162">This sets up source for the "parent" categories, and second source for the "detail" products.</span></span>
1. <span data-ttu-id="a4ec9-163">Затем добавьте в код XAML следующую разметку после закрывающего тега `Window.Resources`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-163">Next, add the following markup to your XAML after the closing `Window.Resources` tag.</span></span>

    [!code-xaml[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/MainWindow.xaml?range=15-26)]

1. <span data-ttu-id="a4ec9-164">Обратите внимание, что для свойства `CategoryId` задано значение `ReadOnly`, поскольку оно назначается базой данных и не может быть изменено.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-164">Note that the `CategoryId` is set to `ReadOnly` because it is assigned by the database and cannot be changed.</span></span>

## <a name="adding-a-details-grid"></a><span data-ttu-id="a4ec9-165">Добавление таблицы сведений</span><span class="sxs-lookup"><span data-stu-id="a4ec9-165">Adding a Details Grid</span></span>

<span data-ttu-id="a4ec9-166">Теперь, когда существует таблица для отображения категорий, можно добавить таблицу сведений для отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-166">Now that the grid exists to display categories, the details grid can be added to show products.</span></span>

**`MainWindow.xaml`**

[!code-xaml[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/MainWindow.xaml?range=27-40)]

<span data-ttu-id="a4ec9-167">Наконец, добавьте кнопку `Save` и привяжите событие Click к `Button_Click`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-167">Finally, add a `Save` button and wire in the click event to `Button_Click`.</span></span>

[!code-xaml[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/MainWindow.xaml?range=41-42)]

<span data-ttu-id="a4ec9-168">Представление конструктора должно выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a4ec9-168">Your design view should look like this:</span></span>

![Снимок экрана конструктора WPF](_static/wpf-tutorial-designer.jpg)

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="a4ec9-170">Добавление кода, обрабатывающего взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="a4ec9-170">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="a4ec9-171">Теперь следует добавить в главное окно несколько обработчиков событий.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-171">It's time to add some event handlers to the main window.</span></span>

1. <span data-ttu-id="a4ec9-172">В окне XAML щелкните элемент **&lt;Window&gt;** , чтобы выбрать главное окно.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-172">In the XAML window, click on the **&lt;Window&gt;** element, to select the main window.</span></span>
1. <span data-ttu-id="a4ec9-173">В окне **Свойства** в правом верхнем углу выберите **События** , а затем дважды щелкните текстовое поле справа от метки **Загружен**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-173">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label.</span></span>

    ![Свойства главного окна](_static/wpf-tutorial-loaded.jpg)

<span data-ttu-id="a4ec9-175">В результате вы перейдете к коду программной части формы, который теперь можно изменить, чтобы использовать `ProductContext` для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-175">This brings you to the code behind for the form, we'll now edit the code to use the `ProductContext` to perform data access.</span></span> <span data-ttu-id="a4ec9-176">Обновите код, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-176">Update the code as shown below.</span></span>

<span data-ttu-id="a4ec9-177">Код объявляет длительный экземпляр `ProductContext`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-177">The code declares a long-running instance of `ProductContext`.</span></span> <span data-ttu-id="a4ec9-178">Объект `ProductContext` используется для запроса и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-178">The `ProductContext` object is used to query and save data to the database.</span></span> <span data-ttu-id="a4ec9-179">Затем из переопределенного метода `OnClosing` вызывается метод `Dispose()` для экземпляра `ProductContext`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-179">The `Dispose()` method on the `ProductContext` instance is then called from the overridden `OnClosing` method.</span></span> <span data-ttu-id="a4ec9-180">Комментарии к коду объясняют, что делает каждый шаг.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-180">The code comments explain what each step does.</span></span>

**`MainWindow.xaml.cs`**

[!code-csharp[](../../../samples/core/WPF/GetStartedWPF/GetStartedWPF/MainWindow.xaml.cs)]

> [!NOTE]
> <span data-ttu-id="a4ec9-181">Для создания базы данных при первом запуске в коде используется вызов метода `EnsureCreated()`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-181">The code uses a call to `EnsureCreated()` to build the database on the first run.</span></span> <span data-ttu-id="a4ec9-182">Это приемлемо для демонстраций, но в рабочих приложениях следует изучить возможности [миграций](xref:core/managing-schemas/migrations/index) для управления схемой.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-182">This is acceptable for demos, but in production apps you should look at [migrations](xref:core/managing-schemas/migrations/index) to manage your schema.</span></span> <span data-ttu-id="a4ec9-183">Код также выполняется синхронно, так как он использует локальную базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-183">The code also executes synchronously because it uses a local SQLite database.</span></span> <span data-ttu-id="a4ec9-184">Для сценариев в рабочей среде, которые обычно предполагают использование удаленного сервера, рекомендуется применять асинхронные версии методов `Load` и `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-184">For production scenarios that typically involve a remote server, consider using the asynchronous versions of the `Load` and `SaveChanges` methods.</span></span>

## <a name="test-the-wpf-application"></a><span data-ttu-id="a4ec9-185">Тестирование приложения WPF</span><span class="sxs-lookup"><span data-stu-id="a4ec9-185">Test the WPF Application</span></span>

<span data-ttu-id="a4ec9-186">Скомпилируйте и запустите приложение, нажав клавишу **F5** или выбрав **Отладка&gt; Начать отладку**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-186">Compile and run the application by pressing **F5** or choosing **Debug &gt; Start Debugging**.</span></span> <span data-ttu-id="a4ec9-187">База данных должна быть создана автоматически и сохранена в файле с именем `products.db`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-187">The database should be automatically created with a file named `products.db`.</span></span> <span data-ttu-id="a4ec9-188">Введите имя категории и нажмите клавишу ВВОД, а затем добавьте продукты в нижнюю таблицу.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-188">Enter a category name and hit enter, then add products to the lower grid.</span></span> <span data-ttu-id="a4ec9-189">Нажмите кнопку "Сохранить" и посмотрите, обновилась ли таблица с учетом указанных идентификаторов базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-189">Click save and watch the grid refresh with the database provided ids.</span></span> <span data-ttu-id="a4ec9-190">Выделите строку и нажмите **Удалить** , чтобы ее удалить.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-190">Highlight a row and hit **Delete** to remove the row.</span></span> <span data-ttu-id="a4ec9-191">Сущность будет удалена при нажатии кнопки **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-191">The entity will be deleted when you click **Save**.</span></span>

![Выполнение приложения](_static/wpf-tutorial-app.jpg)

## <a name="property-change-notification"></a><span data-ttu-id="a4ec9-193">Уведомление об изменении свойства</span><span class="sxs-lookup"><span data-stu-id="a4ec9-193">Property Change Notification</span></span>

<span data-ttu-id="a4ec9-194">Этот пример предполагает выполнение четырех шагов для синхронизации сущностей с пользовательским интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-194">This example relies on four steps to synchronize the entities with the UI.</span></span>

1. <span data-ttu-id="a4ec9-195">При начальном вызове `_context.Categories.Load()` загружаются данные категорий.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-195">The initial call `_context.Categories.Load()` loads the categories data.</span></span>
1. <span data-ttu-id="a4ec9-196">Прокси отложенной загрузки загружают данные зависимых продуктов.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-196">The lazy-loading proxies load the dependent products data.</span></span>
1. <span data-ttu-id="a4ec9-197">Встроенная функция отслеживания изменений EF Core вносит необходимые изменения в сущности, в том числе вставки и удаления, при вызове метода `_context.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-197">EF Core's built-in change tracking makes the necessary modifications to entities, including insertions and deletions, when `_context.SaveChanges()` is called.</span></span>
1. <span data-ttu-id="a4ec9-198">Вызовы `DataGridView.Items.Refresh()` приводят к принудительной перезагрузке с вновь созданными идентификаторами.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-198">The calls to `DataGridView.Items.Refresh()` force a reload with the newly generated ids.</span></span>

<span data-ttu-id="a4ec9-199">Это подходит для нашего примера для начала работы, в других сценариях может потребоваться дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-199">This works for our getting started sample, but you may require additional code for other scenarios.</span></span> <span data-ttu-id="a4ec9-200">Элементы управления WPF преобразовывают для просмотра пользовательский интерфейс, считывая поля и свойства сущностей.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-200">WPF controls render the UI by reading the fields and properties on your entities.</span></span> <span data-ttu-id="a4ec9-201">При изменении значения в пользовательском интерфейсе это значение передается в сущность.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-201">When you edit a value in the user interface (UI), that value is passed to your entity.</span></span> <span data-ttu-id="a4ec9-202">При изменении значения свойства непосредственно в сущности, например при его загрузке из базы данных, WPF не отражает эти изменения сразу же в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-202">When you change the value of a property directly on your entity, such as loading it from the database, WPF will not immediately reflect the changes in the UI.</span></span> <span data-ttu-id="a4ec9-203">Подсистема преобразования для просмотра должна быть уведомлена об изменениях.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-203">The rendering engine must be notified of the changes.</span></span> <span data-ttu-id="a4ec9-204">В данном проекте это выполняется путем вызова `Refresh()` вручную.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-204">The project did this by manually calling `Refresh()`.</span></span> <span data-ttu-id="a4ec9-205">Такое уведомление можно легко автоматизировать, реализовав интерфейс [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-205">An easy way to automate this notification is by implementing the [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) interface.</span></span> <span data-ttu-id="a4ec9-206">Компоненты WPF будут автоматически обнаруживать интерфейс и регистрироваться для получения событий изменения.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-206">WPF components will automatically detect the interface and register for change events.</span></span> <span data-ttu-id="a4ec9-207">За создание таких событий отвечает сущность.</span><span class="sxs-lookup"><span data-stu-id="a4ec9-207">The entity is responsible for raising these events.</span></span>

> [!TIP]
> <span data-ttu-id="a4ec9-208">Дополнительные сведения об обработке изменений см. в статье [Практическое руководство. Реализация уведомления об изменении свойства](/dotnet/framework/wpf/data/how-to-implement-property-change-notification).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-208">To learn more about how to handle changes, read: [How to implement property change notification](/dotnet/framework/wpf/data/how-to-implement-property-change-notification).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4ec9-209">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a4ec9-209">Next Steps</span></span>

<span data-ttu-id="a4ec9-210">Узнайте больше о [настройке DbContext](xref:core/dbcontext-configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a4ec9-210">Learn more about [Configuring a DbContext](xref:core/dbcontext-configuration/index).</span></span>
