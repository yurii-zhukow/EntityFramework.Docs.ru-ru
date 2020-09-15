---
title: Model First EF6
description: Model First в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/designer/workflows/model-first
ms.openlocfilehash: 13c5e173182ad0781a674ce3d10f7374c4443bf9
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073166"
---
# <a name="model-first"></a><span data-ttu-id="1c69a-103">Model First</span><span class="sxs-lookup"><span data-stu-id="1c69a-103">Model First</span></span>
<span data-ttu-id="1c69a-104">В этом видео и пошаговом руководстве представлены общие сведения о Model First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c69a-104">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="1c69a-105">Model First позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-105">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="1c69a-106">Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1c69a-106">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1c69a-107">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="1c69a-107">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1c69a-108">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="1c69a-108">Watch the video</span></span>
<span data-ttu-id="1c69a-109">В этом видео и пошаговом руководстве представлены общие сведения о Model First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c69a-109">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="1c69a-110">Model First позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-110">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="1c69a-111">Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1c69a-111">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="1c69a-112">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="1c69a-112">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="1c69a-113">**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="1c69a-113">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="1c69a-114">**Видео**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv)  |  [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="1c69a-114">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1c69a-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1c69a-115">Pre-Requisites</span></span>

<span data-ttu-id="1c69a-116">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1c69a-116">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="1c69a-117">Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="1c69a-117">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="1c69a-118">1. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="1c69a-118">1. Create the Application</span></span>

<span data-ttu-id="1c69a-119">Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Model First для доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="1c69a-119">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="1c69a-120">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c69a-120">Open Visual Studio</span></span>
-   <span data-ttu-id="1c69a-121">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-121">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="1c69a-122">Выбор **окон** в меню слева и **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="1c69a-122">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="1c69a-123">Введите **моделфирстсампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="1c69a-123">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="1c69a-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1c69a-124">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="1c69a-125">2. Создание модели</span><span class="sxs-lookup"><span data-stu-id="1c69a-125">2. Create Model</span></span>

<span data-ttu-id="1c69a-126">Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-126">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="1c69a-127">**Проект- &gt; Добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-127">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="1c69a-128">Выберите **данные** в меню слева, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="1c69a-128">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1c69a-129">Введите **блоггингмодел** в качестве имени и нажмите кнопку " **ОК**", чтобы запустить мастер EDM</span><span class="sxs-lookup"><span data-stu-id="1c69a-129">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="1c69a-130">Выберите **пустую модель** и нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-130">Select **Empty Model** and click **Finish**</span></span>

    ![Создать пустую модель](~/ef6/media/createemptymodel.png)

<span data-ttu-id="1c69a-132">Entity Framework Designer открывается с пустой моделью.</span><span class="sxs-lookup"><span data-stu-id="1c69a-132">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="1c69a-133">Теперь можно приступить к добавлению сущностей, свойств и ассоциаций в модель.</span><span class="sxs-lookup"><span data-stu-id="1c69a-133">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="1c69a-134">Щелкните правой кнопкой мыши область конструктора и выберите пункт **Свойства** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-134">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="1c69a-135">В окно свойств изменить **имя контейнера сущностей** на **BloggingContext** 
     *это имя производного контекста, который будет создан автоматически, контекст представляет сеанс с базой данных, что позволяет запрашивать и сохранять данные*</span><span class="sxs-lookup"><span data-stu-id="1c69a-135">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="1c69a-136">Щелкните правой кнопкой мыши область конструктора и выберите команду **Добавить новую &gt; сущность...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-136">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="1c69a-137">Введите в качестве имени сущности **блог** и **блогид** в качестве имени ключа и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-137">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Добавить сущность блога](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="1c69a-139">Щелкните правой кнопкой мыши новую сущность в области конструктора и выберите **Добавить новое &gt; скалярное свойство**, введите **Name** в качестве имени свойства.</span><span class="sxs-lookup"><span data-stu-id="1c69a-139">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="1c69a-140">Повторите эту процедуру, чтобы добавить свойство **URL-адреса** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-140">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="1c69a-141">Щелкните правой кнопкой мыши свойство **URL-адрес** в области конструктора и выберите **пункт свойства**, в окно свойств изменить значение параметра **Nullable** на **true**, что 
     *позволит нам сохранить блог в базе данных, не назначив ему URL-адрес* .</span><span class="sxs-lookup"><span data-stu-id="1c69a-141">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="1c69a-142">Используя только что знакомые методы, добавьте сущность **POST** со свойством ключа **постид** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-142">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="1c69a-143">Добавление скалярных свойств **Title** и **Content** в сущность **POST**</span><span class="sxs-lookup"><span data-stu-id="1c69a-143">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="1c69a-144">Теперь, когда у нас есть пара сущностей, пришло время добавить ассоциацию (или связь) между ними.</span><span class="sxs-lookup"><span data-stu-id="1c69a-144">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="1c69a-145">Щелкните правой кнопкой мыши область конструктора и выберите команду **Добавить новую &gt; связь...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-145">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="1c69a-146">Сделать один конец точки связи в **блоге** , используя кратность **одной** и другой конечной точки для **публикации** с кратностью. **Many** 
     *это означает, что в блоге имеется много записей, а запись принадлежит одному блогу* .</span><span class="sxs-lookup"><span data-stu-id="1c69a-146">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="1c69a-147">Убедитесь, что флажок **Добавить свойства внешнего ключа в сущность ' POST '** установлен, и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-147">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Добавление ассоциации MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="1c69a-149">Теперь у нас есть простая модель, из которой можно создать базу данных и использовать ее для чтения и записи данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-149">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="1c69a-151">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="1c69a-151">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="1c69a-152">При работе в Visual Studio 2010 необходимо выполнить некоторые дополнительные действия по обновлению до последней версии Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c69a-152">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="1c69a-153">Обновление важно, так как оно предоставляет доступ к улучшенной поверхности API, которая гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="1c69a-153">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="1c69a-154">Сначала необходимо получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="1c69a-154">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="1c69a-155">**Проект — &gt; Управление пакетами NuGet...** 
     *Если у вас нет параметра **Управление пакетами NuGet** , следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*</span><span class="sxs-lookup"><span data-stu-id="1c69a-155">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="1c69a-156">Выбор вкладки "в **сети** "</span><span class="sxs-lookup"><span data-stu-id="1c69a-156">Select the **Online** tab</span></span>
-   <span data-ttu-id="1c69a-157">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="1c69a-157">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="1c69a-158">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="1c69a-158">Click **Install**</span></span>

<span data-ttu-id="1c69a-159">Далее необходимо поменять модель, чтобы создать код, который использует API DbContext, который появился в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c69a-159">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="1c69a-160">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-160">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="1c69a-161">В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**</span><span class="sxs-lookup"><span data-stu-id="1c69a-161">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="1c69a-162">Выберите генератор EF \*\*5. x DbContext для C \# \*\*, введите **блоггингмодел** в качестве имени и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-162">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="1c69a-164">3. Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="1c69a-164">3. Generating the Database</span></span>

<span data-ttu-id="1c69a-165">С учетом нашей модели Entity Framework может вычислить схему базы данных, которая позволит хранить и получать данные с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-165">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="1c69a-166">Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c69a-166">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1c69a-167">При использовании Visual Studio 2010 будет создаваться база данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="1c69a-167">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="1c69a-168">Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="1c69a-168">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="1c69a-169">Давайте создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-169">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1c69a-170">Щелкните правой кнопкой мыши область конструктора и выберите команду **создать базу данных из модели...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-170">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="1c69a-171">Нажмите кнопку **создать соединение...**</span><span class="sxs-lookup"><span data-stu-id="1c69a-171">Click **New Connection…**</span></span> <span data-ttu-id="1c69a-172">и укажите либо LocalDB, либо SQL Express, в зависимости от используемой версии Visual Studio введите **моделфирст. блоги** в качестве имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-172">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Подключение к LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Подключение к SQL Express (MF)](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="1c69a-175">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-175">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="1c69a-176">Нажмите кнопку **Далее** , и Entity Framework Designer вычислит сценарий для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-176">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="1c69a-177">После отображения скрипта нажмите кнопку **Готово** , и скрипт будет добавлен в проект и открыт.</span><span class="sxs-lookup"><span data-stu-id="1c69a-177">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="1c69a-178">Щелкните скрипт правой кнопкой мыши и выберите команду **выполнить**. вам будет предложено указать базу данных для подключения, указать LocalDB или SQL Server Express в зависимости от используемой версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c69a-178">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="1c69a-179">4. чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="1c69a-179">4. Reading & Writing Data</span></span>

<span data-ttu-id="1c69a-180">Теперь, когда у нас есть модель, пришло время использовать ее для доступа к некоторым данным.</span><span class="sxs-lookup"><span data-stu-id="1c69a-180">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="1c69a-181">Классы, которые мы будем использовать для доступа к данным, создаются автоматически на основе файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="1c69a-181">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="1c69a-182">*Этот снимок экрана находится в Visual Studio 2012, если вы используете Visual Studio 2010 файлы BloggingModel.tt и BloggingModel.Context.tt будут находиться непосредственно в проекте, а не вложены в EDMX-файл.*</span><span class="sxs-lookup"><span data-stu-id="1c69a-182">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Созданные классы](~/ef6/media/generatedclasses.png)

<span data-ttu-id="1c69a-184">Реализуйте метод Main в Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1c69a-184">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="1c69a-185">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="1c69a-185">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="1c69a-186">Затем он использует запрос LINQ для получения всех блогов из базы данных, упорядоченного в алфавитном порядке по названию.</span><span class="sxs-lookup"><span data-stu-id="1c69a-186">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="1c69a-187">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="1c69a-187">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="1c69a-188">5. Работа с изменениями модели</span><span class="sxs-lookup"><span data-stu-id="1c69a-188">5. Dealing with Model Changes</span></span>

<span data-ttu-id="1c69a-189">Пришло время внести некоторые изменения в нашу модель, при внесении этих изменений нам также потребуется обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-189">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="1c69a-190">Начнем с добавления новой сущности пользователя в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="1c69a-190">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="1c69a-191">Добавьте новое имя сущности **пользователя** с именем **пользователя** в качестве имени ключа и **строки** в качестве типа свойства для ключа.</span><span class="sxs-lookup"><span data-stu-id="1c69a-191">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Добавить сущность пользователя](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="1c69a-193">Щелкните правой кнопкой мыши свойство **username** в области конструктора и выберите пункт **свойства**, в окно свойств изменить значение параметра **MaxLength** на 50. **50** 
     *это ограничит данные, которые могут храниться в имени пользователя, до 50 символов* .</span><span class="sxs-lookup"><span data-stu-id="1c69a-193">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="1c69a-194">Добавление скалярного свойства **DisplayName** в сущность **User**</span><span class="sxs-lookup"><span data-stu-id="1c69a-194">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="1c69a-195">Теперь у нас есть обновленная модель, и мы готовы к обновлению базы данных для размещения нового типа сущности пользователя.</span><span class="sxs-lookup"><span data-stu-id="1c69a-195">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="1c69a-196">Щелкните правой кнопкой мыши область конструктора и выберите команду **создать базу данных из модели...** Entity Framework вычислит скрипт для повторного создания схемы на основе обновленной модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-196">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="1c69a-197">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="1c69a-197">Click **Finish**</span></span>
-   <span data-ttu-id="1c69a-198">Вы можете получить предупреждения о перезаписи существующего скрипта DDL, а также части сопоставления и хранилища модели. для этих предупреждений нажмите кнопку **Да** .</span><span class="sxs-lookup"><span data-stu-id="1c69a-198">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="1c69a-199">Обновленный скрипт SQL для создания базы данных будет открыт для вас.</span><span class="sxs-lookup"><span data-stu-id="1c69a-199">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="1c69a-200">*Созданный скрипт удалит все существующие таблицы, а затем воссоздаст схему с нуля. Это может работать для локальной разработки, но не для принудительной отправки изменений в базу данных, которая уже была развернута. Если необходимо опубликовать изменения в базе данных, которая уже была развернута, необходимо изменить скрипт или использовать средство сравнения схем для вычисления скрипта миграции.*</span><span class="sxs-lookup"><span data-stu-id="1c69a-200">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="1c69a-201">Щелкните скрипт правой кнопкой мыши и выберите команду **выполнить**. вам будет предложено указать базу данных для подключения, указать LocalDB или SQL Server Express в зависимости от используемой версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c69a-201">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="1c69a-202">Сводка</span><span class="sxs-lookup"><span data-stu-id="1c69a-202">Summary</span></span>

<span data-ttu-id="1c69a-203">В этом пошаговом руководстве мы рассматривали Model Firstную разработку, которая позволила нам создать модель в конструкторе EF, а затем создать базу данных из этой модели.</span><span class="sxs-lookup"><span data-stu-id="1c69a-203">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="1c69a-204">Затем мы использовали эту модель для чтения и записи данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1c69a-204">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="1c69a-205">Наконец, мы обновили модель, а затем воссоздали схему базы данных в соответствии с моделью.</span><span class="sxs-lookup"><span data-stu-id="1c69a-205">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
