---
title: Model First EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182446"
---
# <a name="model-first"></a><span data-ttu-id="020be-102">Model First</span><span class="sxs-lookup"><span data-stu-id="020be-102">Model First</span></span>
<span data-ttu-id="020be-103">В этом видео и пошаговом руководстве представлены общие сведения о Model First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="020be-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="020be-104">Model First позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="020be-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="020be-105">Модель хранится в EDMX-файле (расширение .emdx), и её можно просмотреть и изменить в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="020be-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="020be-106">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="020be-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="020be-107">Просмотрите видео</span><span class="sxs-lookup"><span data-stu-id="020be-107">Watch the video</span></span>
<span data-ttu-id="020be-108">В этом видео и пошаговом руководстве представлены общие сведения о Model First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="020be-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="020be-109">Model First позволяет создать новую модель с помощью Entity Framework Designer, а затем создать схему базы данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="020be-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="020be-110">Модель хранится в EDMX-файле (расширение .emdx), и её можно просмотреть и изменить в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="020be-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="020be-111">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="020be-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="020be-112">**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="020be-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="020be-113">**Видео**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="020be-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="020be-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="020be-114">Pre-Requisites</span></span>

<span data-ttu-id="020be-115">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="020be-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="020be-116">Если вы используете Visual Studio 2010, также необходимо будет установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="020be-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="020be-117">1. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="020be-117">1. Create the Application</span></span>

<span data-ttu-id="020be-118">Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Model First для доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="020be-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="020be-119">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="020be-119">Open Visual Studio</span></span>
-   <span data-ttu-id="020be-120">**Файл —&gt; Создать —&gt; Проект…**</span><span class="sxs-lookup"><span data-stu-id="020be-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="020be-121">В меню слева выберите **Windows** и **Консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="020be-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="020be-122">Введите **моделфирстсампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="020be-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="020be-123">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="020be-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="020be-124">2. Создание модели</span><span class="sxs-lookup"><span data-stu-id="020be-124">2. Create Model</span></span>

<span data-ttu-id="020be-125">Для создания нашей модели мы собираемся использовать конструктор Entity Framework Designer, который входит в состав Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="020be-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="020be-126">**Проект -&gt; Добавить новый элемент…**</span><span class="sxs-lookup"><span data-stu-id="020be-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="020be-127">Выберите **Данные** в меню слева и затем **Модель ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="020be-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="020be-128">Введите **блоггингмодел** в качестве имени и нажмите кнопку " **ОК**", чтобы запустить мастер EDM</span><span class="sxs-lookup"><span data-stu-id="020be-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="020be-129">Выберите **пустую модель** и нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="020be-129">Select **Empty Model** and click **Finish**</span></span>

    ![Создать пустую модель](~/ef6/media/createemptymodel.png)

<span data-ttu-id="020be-131">Entity Framework Designer открывается с пустой моделью.</span><span class="sxs-lookup"><span data-stu-id="020be-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="020be-132">Теперь можно приступить к добавлению сущностей, свойств и ассоциаций в модель.</span><span class="sxs-lookup"><span data-stu-id="020be-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="020be-133">Щелкните правой кнопкой мыши область конструктора и выберите пункт **Свойства** .</span><span class="sxs-lookup"><span data-stu-id="020be-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="020be-134">В окно свойств изменить **имя контейнера сущностей** на **BloggingContext**
    *это имя производного контекста, который будет создан автоматически, контекст представляет сеанс с базой данных, что позволяет запрашивать и сохранять данные*</span><span class="sxs-lookup"><span data-stu-id="020be-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="020be-135">Щелкните правой кнопкой мыши область конструктора и выберите команду **Добавить новую&gt; сущность...**</span><span class="sxs-lookup"><span data-stu-id="020be-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="020be-136">Введите в качестве имени сущности **блог** и **блогид** в качестве имени ключа и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="020be-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Добавить сущность блога](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="020be-138">Щелкните правой кнопкой мыши новую сущность в области конструктора и выберите **Добавить New-&gt; скалярное свойство**, введите **Name** в качестве имени свойства.</span><span class="sxs-lookup"><span data-stu-id="020be-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="020be-139">Повторите эту процедуру, чтобы добавить свойство **URL-адреса** .</span><span class="sxs-lookup"><span data-stu-id="020be-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="020be-140">Щелкните правой кнопкой мыши свойство **URL-адрес** в области конструктора и выберите пункт **свойства**, в окно свойств изменить значение параметра **Nullable** на **true**
    *это позволит нам сохранить блог в базе данных, не назначив ему URL-адрес* .</span><span class="sxs-lookup"><span data-stu-id="020be-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="020be-141">Используя только что знакомые методы, добавьте сущность **POST** со свойством ключа **постид** .</span><span class="sxs-lookup"><span data-stu-id="020be-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="020be-142">Добавление скалярных свойств **Title** и **Content** в сущность **POST**</span><span class="sxs-lookup"><span data-stu-id="020be-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="020be-143">Теперь, когда у нас есть пара сущностей, пришло время добавить ассоциацию (или связь) между ними.</span><span class="sxs-lookup"><span data-stu-id="020be-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="020be-144">Щелкните правой кнопкой мыши область конструктора и выберите команду **Добавить новую&gt; ассоциацию...**</span><span class="sxs-lookup"><span data-stu-id="020be-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="020be-145">Сделать один конец точки связи в **блоге** , используя кратность **одной** и другой конечной точки для **публикации** с кратностью **множества**
    *это означает, что в блоге имеется много записей, а запись принадлежит одному блогу* .</span><span class="sxs-lookup"><span data-stu-id="020be-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="020be-146">Убедитесь, что флажок **Добавить свойства внешнего ключа в сущность ' POST '** установлен, и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="020be-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Добавление ассоциации MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="020be-148">Теперь у нас есть простая модель, из которой можно создать базу данных и использовать ее для чтения и записи данных.</span><span class="sxs-lookup"><span data-stu-id="020be-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="020be-150">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="020be-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="020be-151">При работе в Visual Studio 2010 необходимо выполнить некоторые дополнительные действия по обновлению до последней версии Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="020be-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="020be-152">Обновление важно, так как оно предоставляет вам доступ к улучшенным API, которые гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="020be-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="020be-153">Сначала необходимо получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="020be-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="020be-154">\*\*Проект —&gt; Управление пакетами NuGet... \*\* 
     \*При отсутствии пункта \**Управление пакетами NuGet… \*\* следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="020be-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="020be-155">Выберите вкладку **В сети**</span><span class="sxs-lookup"><span data-stu-id="020be-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="020be-156">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="020be-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="020be-157">Нажмите кнопку **Установить**</span><span class="sxs-lookup"><span data-stu-id="020be-157">Click **Install**</span></span>

<span data-ttu-id="020be-158">Далее нам нужно переключить нашу модель, чтобы сгенерировать код, который использует API DbContext, появившийся в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="020be-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="020be-159">Щелкните правой кнопкой мыши на пустом месте модели в EF Designer и выберите **Добавить элемент создания кода…**</span><span class="sxs-lookup"><span data-stu-id="020be-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="020be-160">Выберите **Шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="020be-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="020be-161">Выберите генератор EF **5. x DbContext для C\#** , введите **блоггингмодел** в качестве имени и нажмите кнопку **добавить** .</span><span class="sxs-lookup"><span data-stu-id="020be-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="020be-163">3. Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="020be-163">3. Generating the Database</span></span>

<span data-ttu-id="020be-164">С учетом нашей модели Entity Framework может вычислить схему базы данных, которая позволит хранить и получать данные с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="020be-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="020be-165">Сервер базы данных, который устанавливается вместе с Visual Studio, отличается в зависимости от версии Visual Studio, которую вы установили:</span><span class="sxs-lookup"><span data-stu-id="020be-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="020be-166">Если вы используете Visual Studio 2010, вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="020be-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="020be-167">Если вы используете Visual Studio 2012, вы создадите базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="020be-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="020be-168">Перейдем дальше и создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="020be-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="020be-169">Щелкните правой кнопкой мыши область конструктора и выберите команду **создать базу данных из модели...**</span><span class="sxs-lookup"><span data-stu-id="020be-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="020be-170">Нажмите кнопку **создать соединение...**</span><span class="sxs-lookup"><span data-stu-id="020be-170">Click **New Connection…**</span></span> <span data-ttu-id="020be-171">и укажите либо LocalDB, либо SQL Express, в зависимости от используемой версии Visual Studio введите **моделфирст. блоги** в качестве имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="020be-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Подключение к LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Подключение к SQL Express (MF)](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="020be-174">Выберите **ОК**, и вам будет задан вопрос, хотите ли вы создать новую базу данных. Выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="020be-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="020be-175">Нажмите кнопку **Далее** , и Entity Framework Designer вычислит сценарий для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="020be-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="020be-176">После отображения скрипта нажмите кнопку **Готово** , и скрипт будет добавлен в проект и открыт.</span><span class="sxs-lookup"><span data-stu-id="020be-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="020be-177">Щелкните скрипт правой кнопкой мыши и выберите команду **выполнить**. вам будет предложено указать базу данных для подключения, указать LocalDB или SQL Server Express в зависимости от используемой версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="020be-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="020be-178">4. Чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="020be-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="020be-179">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к каким-нибудь данным.</span><span class="sxs-lookup"><span data-stu-id="020be-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="020be-180">Классы, которые мы будем использовать для доступа к данным, автоматически создаются в зависимости от EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="020be-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="020be-181">*Этот снимок экрана из Visual Studio 2012. Если вы используете Visual Studio 2010, файлы BloggingModel.tt и BloggingModel.Context.tt будут лежать непосредственно в проекте, а не вложены в узел EDMX-файла.*</span><span class="sxs-lookup"><span data-stu-id="020be-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Созданные классы](~/ef6/media/generatedclasses.png)

<span data-ttu-id="020be-183">Реализуйте метод Main в файле Program.cs так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="020be-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="020be-184">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки новой записи блога.</span><span class="sxs-lookup"><span data-stu-id="020be-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="020be-185">Затем он использует запрос LINQ для извлечения из базы данных всех записей блога, упорядоченных в алфавитном порядке по названию.</span><span class="sxs-lookup"><span data-stu-id="020be-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="020be-186">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="020be-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="020be-187">5. Работа с изменениями модели</span><span class="sxs-lookup"><span data-stu-id="020be-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="020be-188">Пришло время внести некоторые изменения в нашу модель, при внесении этих изменений нам также потребуется обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="020be-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="020be-189">Начнем с добавления новой сущности пользователя в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="020be-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="020be-190">Добавьте новое имя сущности **пользователя** с именем **пользователя** в качестве имени ключа и **строки** в качестве типа свойства для ключа.</span><span class="sxs-lookup"><span data-stu-id="020be-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Добавить сущность пользователя](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="020be-192">Щелкните правой кнопкой мыши свойство **username** в области конструктора и выберите пункт **свойства**, в окно свойств изменить значение параметра **MaxLength** на **50**
    *это ограничит данные, которые могут храниться в имени пользователя, до 50 символов* .</span><span class="sxs-lookup"><span data-stu-id="020be-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="020be-193">Добавление скалярного свойства **DisplayName** в сущность **User**</span><span class="sxs-lookup"><span data-stu-id="020be-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="020be-194">Теперь у нас есть обновленная модель, и мы готовы к обновлению базы данных для размещения нового типа сущности пользователя.</span><span class="sxs-lookup"><span data-stu-id="020be-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="020be-195">Щелкните правой кнопкой мыши область конструктора и выберите команду **создать базу данных из модели...** Entity Framework вычислит скрипт для повторного создания схемы на основе обновленной модели.</span><span class="sxs-lookup"><span data-stu-id="020be-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="020be-196">Нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="020be-196">Click **Finish**</span></span>
-   <span data-ttu-id="020be-197">Вы можете получить предупреждения о перезаписи существующего скрипта DDL, а также части сопоставления и хранилища модели. для этих предупреждений нажмите кнопку **Да** .</span><span class="sxs-lookup"><span data-stu-id="020be-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="020be-198">Обновленный скрипт SQL для создания базы данных будет открыт для вас.</span><span class="sxs-lookup"><span data-stu-id="020be-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="020be-199">*Созданный скрипт удалит все существующие таблицы, а затем воссоздаст схему с нуля. Это может работать для локальной разработки, но не для принудительной отправки изменений в базу данных, которая уже была развернута. Если необходимо опубликовать изменения в базе данных, которая уже была развернута, необходимо изменить скрипт или использовать средство сравнения схем для вычисления скрипта миграции.*</span><span class="sxs-lookup"><span data-stu-id="020be-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="020be-200">Щелкните скрипт правой кнопкой мыши и выберите команду **выполнить**. вам будет предложено указать базу данных для подключения, указать LocalDB или SQL Server Express в зависимости от используемой версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="020be-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="020be-201">Сводка</span><span class="sxs-lookup"><span data-stu-id="020be-201">Summary</span></span>

<span data-ttu-id="020be-202">В этом пошаговом руководстве мы рассматривали Model Firstную разработку, которая позволила нам создать модель в конструкторе EF, а затем создать базу данных из этой модели.</span><span class="sxs-lookup"><span data-stu-id="020be-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="020be-203">Затем мы использовали эту модель для чтения и записи данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="020be-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="020be-204">Наконец, мы обновили модель, а затем воссоздали схему базы данных в соответствии с моделью.</span><span class="sxs-lookup"><span data-stu-id="020be-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
