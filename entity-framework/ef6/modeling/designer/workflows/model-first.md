---
title: Model First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122270"
---
# <a name="model-first"></a><span data-ttu-id="5b7c6-102">Model First</span><span class="sxs-lookup"><span data-stu-id="5b7c6-102">Model First</span></span>
<span data-ttu-id="5b7c6-103">В этом пошаговом руководстве видео и пошаговые познакомят вас с первой модели разработки, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="5b7c6-104">Во-первых, модель позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных из модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="5b7c6-105">Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="5b7c6-106">Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="5b7c6-107">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="5b7c6-107">Watch the video</span></span>
<span data-ttu-id="5b7c6-108">В этом пошаговом руководстве видео и пошаговые познакомят вас с первой модели разработки, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="5b7c6-109">Во-первых, модель позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных из модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="5b7c6-110">Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="5b7c6-111">Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="5b7c6-112">**Представляет**: [Роуэн Миллер (Rowan Miller)](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="5b7c6-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="5b7c6-113">**Видео**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="5b7c6-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5b7c6-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5b7c6-114">Pre-Requisites</span></span>

<span data-ttu-id="5b7c6-115">Для выполнения этого пошагового руководства вам потребуется Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="5b7c6-116">Если вы используете Visual Studio 2010, также необходимо будет иметь [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="5b7c6-117">1. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="5b7c6-117">1. Create the Application</span></span>

<span data-ttu-id="5b7c6-118">Для простоты мы создадим простое консольное приложение, использующего Model First для осуществления доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="5b7c6-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="5b7c6-119">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b7c6-119">Open Visual Studio</span></span>
-   <span data-ttu-id="5b7c6-120">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="5b7c6-121">Выберите **Windows** в меню слева и **консольного приложения**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="5b7c6-122">Введите **ModelFirstSample** как имя</span><span class="sxs-lookup"><span data-stu-id="5b7c6-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="5b7c6-123">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="5b7c6-124">2. Создание модели</span><span class="sxs-lookup"><span data-stu-id="5b7c6-124">2. Create Model</span></span>

<span data-ttu-id="5b7c6-125">Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="5b7c6-126">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="5b7c6-127">Выберите **данных** в меню слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="5b7c6-128">Введите **BloggingModel** имя и нажмите кнопку **ОК**, запустится мастер моделей EDM</span><span class="sxs-lookup"><span data-stu-id="5b7c6-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="5b7c6-129">Выберите **пустую модель** и нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-129">Select **Empty Model** and click **Finish**</span></span>

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

<span data-ttu-id="5b7c6-131">Entity Framework Designer открывается с пустой модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="5b7c6-132">Теперь мы сможем добавлять сущности, свойства и ассоциации в модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="5b7c6-133">Щелкните правой кнопкой мыши на область конструктора и выберите **свойства**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="5b7c6-134">В окне изменения свойства **имени контейнера сущностей** для **BloggingContext**
    *имя производного контекста, который будет создан автоматически, контекст Представляет сеанс с базой данных, позволяет запрашивать и сохранять данные*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="5b7c6-135">Щелкните правой кнопкой мыши на область конструктора и выберите **Add New -&gt; сущности...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="5b7c6-136">Введите **блог** как имя сущности и **BlogId** имя ключа и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="5b7c6-138">Щелкните правой кнопкой мыши на новую сущность в область конструктора и выберите **Add New -&gt; скалярное свойство**, введите **имя** как имя свойства.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="5b7c6-139">Повторите эту процедуру, чтобы добавить **URL-адрес** свойство.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="5b7c6-140">Щелкните правой кнопкой мыши **URL-адрес** свойство в область конструктора и выберите **свойства**, в окне изменения свойства **Nullable** присвоить **True** 
     *Это позволяет сохранить блог в базе данных, не назначая его URL-адрес*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="5b7c6-141">С помощью методик, вы только что ознакомились предварительными сведениями, добавьте **Post** сущность с **PostId** ключевое свойство</span><span class="sxs-lookup"><span data-stu-id="5b7c6-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="5b7c6-142">Добавить **Title** и **содержимого** скалярные свойства **Post** сущности</span><span class="sxs-lookup"><span data-stu-id="5b7c6-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="5b7c6-143">Теперь, когда у нас есть несколько сущностей, пришло время добавить сопоставление (или связь) между ними.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="5b7c6-144">Щелкните правой кнопкой мыши на область конструктора и выберите **Add New -&gt; связи...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="5b7c6-145">Сделать один конец связи пункты **блог** с кратность **один** и других конечную точку для **Post** с кратность **многих** 
     *Это означает, что блог записях и Post принадлежит один блог*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="5b7c6-146">Убедитесь, **добавить свойства внешнего ключа для сущности «Post»** установлен флажок и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="5b7c6-148">Теперь у нас есть простой модели, которую можно создать базу данных из и использовать для чтения и записи данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="5b7c6-150">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5b7c6-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="5b7c6-151">Если вы работаете в Visual Studio 2010, существуют некоторые дополнительные действия, которые необходимо выполнить для обновления до последней версии платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="5b7c6-152">Обновление важно, так как он предоставляет вам доступ к Улучшенная поверхность API, то есть гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="5b7c6-153">Во-первых, нам нужно получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="5b7c6-154">**Проект —&gt; управление пакетами NuGet... ** 
     *При отсутствии **управление пакетами NuGet... ** следует установить параметр [последнюю версию NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="5b7c6-155">Выберите **Online** вкладку</span><span class="sxs-lookup"><span data-stu-id="5b7c6-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="5b7c6-156">Выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="5b7c6-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="5b7c6-157">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-157">Click **Install**</span></span>

<span data-ttu-id="5b7c6-158">Далее нам нужно переключить нашей модели, для которого создается код, который использует API DbContext, который появился в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="5b7c6-159">Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="5b7c6-160">Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="5b7c6-161">Выберите EF **5.x генератор DbContext для C\#**, введите **BloggingModel** имя и нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="5b7c6-163">3. Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="5b7c6-163">3. Generating the Database</span></span>

<span data-ttu-id="5b7c6-164">Учитывая нашу модель, Entity Framework можно вычислить схему базы данных, которые позволят нам для хранения и извлечения данных с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="5b7c6-165">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="5b7c6-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5b7c6-166">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="5b7c6-167">Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="5b7c6-168">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="5b7c6-169">Щелкните правой кнопкой мыши на область конструктора и выберите **создать базу данных из модели...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="5b7c6-170">Нажмите кнопку **новое соединение...**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-170">Click **New Connection…**</span></span> <span data-ttu-id="5b7c6-171">и укажите LocalDB или SQL Express, в зависимости от версии Visual Studio вы используете, введите **ModelFirst.Blogging** имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="5b7c6-174">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="5b7c6-175">Выберите **Далее** и Entity Framework Designer вычислит скрипт для создания схемы базы данных</span><span class="sxs-lookup"><span data-stu-id="5b7c6-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="5b7c6-176">Если он отображается, нажмите кнопку **Готово** , сценарий будет добавлен в проект и открывается</span><span class="sxs-lookup"><span data-stu-id="5b7c6-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="5b7c6-177">Щелкните правой кнопкой мыши сценарий, а затем выберите **Execute**, вам будет предложено указать базу данных для подключения, укажите LocalDB или SQL Server Express, в зависимости от версии Visual Studio вы используете</span><span class="sxs-lookup"><span data-stu-id="5b7c6-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="5b7c6-178">4. Чтение и запись данных</span><span class="sxs-lookup"><span data-stu-id="5b7c6-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="5b7c6-179">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к некоторые данные.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="5b7c6-180">Классы мы будем использовать для доступа к данным автоматически создаются зависимости в EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="5b7c6-181">*Этот снимок экрана из Visual Studio 2012, если вы используете Visual Studio 2010 BloggingModel.tt и BloggingModel.Context.tt файлов будет непосредственно в проекте, а не вложен в узел EDMX-файла.*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClasses](~/ef6/media/generatedclasses.png)

<span data-ttu-id="5b7c6-183">Реализуйте метод Main в файле Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="5b7c6-184">Этот код создает новый экземпляр класса наш контекст, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="5b7c6-185">Затем он использует запрос LINQ для извлечения из базы данных, представлены в алфавитном порядке по названию все блоги.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="5b7c6-186">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="5b7c6-187">5. Изменения модели</span><span class="sxs-lookup"><span data-stu-id="5b7c6-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="5b7c6-188">Теперь пора внести некоторые изменения в нашей модели, когда мы внести эти изменения, необходимо также обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="5b7c6-189">Мы начнем с добавления новой сущности пользователя для нашей модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="5b7c6-190">Добавьте новый **пользователя** имя сущности с **Username** как имя ключа и **строка** как тип свойства для ключа</span><span class="sxs-lookup"><span data-stu-id="5b7c6-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="5b7c6-192">Щелкните правой кнопкой мыши **Username** свойство в область конструктора и выберите **свойства**, изменение свойства окна **MaxLength** присвоить значение **50 ** 
     *Это ограничивает данные, которые могут быть сохранены в имени пользователя до 50 символов*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="5b7c6-193">Добавить **DisplayName** скалярного свойства к **пользователя** сущности</span><span class="sxs-lookup"><span data-stu-id="5b7c6-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="5b7c6-194">Теперь у нас есть обновленной модели, и мы готовы обновить базу данных в соответствии с наш новый тип сущности пользователя.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="5b7c6-195">Щелкните правой кнопкой мыши на область конструктора и выберите **создать базу данных из модели...** , Entity Framework будет вычислять скрипт для повторного создания схемы на основе обновленной модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="5b7c6-196">Нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="5b7c6-196">Click **Finish**</span></span>
-   <span data-ttu-id="5b7c6-197">Можно получать предупреждения о перезаписи существующего скрипта DDL и сопоставления и хранения части модели, щелкните **Да** для обоих этих предупреждений</span><span class="sxs-lookup"><span data-stu-id="5b7c6-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="5b7c6-198">Обновленный сценарий SQL, чтобы создать базу данных открывается при</span><span class="sxs-lookup"><span data-stu-id="5b7c6-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="5b7c6-199">*Созданный скрипт будет удалять все существующие таблицы и заново создать схему с нуля. Это может работать для локальной разработки, но не является приемлемым для принудительной отправки изменений в базе данных, который уже был развернут. Если вам нужно опубликовать изменения в уже развернутой базы данных, необходимо будет изменить сценарий или использовать средство сравнения схем для вычисления скрипт переноса.*</span><span class="sxs-lookup"><span data-stu-id="5b7c6-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="5b7c6-200">Щелкните правой кнопкой мыши сценарий, а затем выберите **Execute**, вам будет предложено указать базу данных для подключения, укажите LocalDB или SQL Server Express, в зависимости от версии Visual Studio вы используете</span><span class="sxs-lookup"><span data-stu-id="5b7c6-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="5b7c6-201">Сводка</span><span class="sxs-lookup"><span data-stu-id="5b7c6-201">Summary</span></span>

<span data-ttu-id="5b7c6-202">В этом пошаговом руководстве мы рассмотрели сначала модель разработки, которое позволяет создать модель в конструкторе EF, а затем создать базу данных из этой модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="5b7c6-203">Затем мы использовали модель для чтения и записи некоторых данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="5b7c6-204">Наконец мы обновившего модель и создать повторно схему базы данных для соответствия модели.</span><span class="sxs-lookup"><span data-stu-id="5b7c6-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
