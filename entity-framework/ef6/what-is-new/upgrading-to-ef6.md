---
title: Обновление до Entity Framework 6 — EF6
description: Обновление до Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
uid: ef6/what-is-new/upgrading-to-ef6
ms.openlocfilehash: b27504e3b18c668a1c41142b9cd5e697944cb5fa
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619710"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="9580e-103">Обновление до Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9580e-103">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="9580e-104">В предыдущих версиях EF код был разделен между основными библиотеками (в основном System.Data.Entity.dll), поставляемыми в составе .NET Framework и нестандартных (OOB) библиотек (в основном EntityFramework.dll), поставляемых в пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="9580e-104">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="9580e-105">EF6 берет код из основных библиотек и внедряет его в библиотеки OOB.</span><span class="sxs-lookup"><span data-stu-id="9580e-105">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="9580e-106">Это требовалось для того, чтобы платформа EF стала открытым исходным кодом и может развиваться в другом темпе от .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9580e-106">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="9580e-107">Следствием этого является то, что приложения потребуется перестраивать для перемещенных типов.</span><span class="sxs-lookup"><span data-stu-id="9580e-107">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="9580e-108">Это должно быть простым для приложений, которые используют DbContext как поставляемые в EF 4,1 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="9580e-108">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="9580e-109">Для приложений, использующих контекст ObjectContext, требуется немного больше работы, но это не так уж сложно.</span><span class="sxs-lookup"><span data-stu-id="9580e-109">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="9580e-110">Ниже приведен контрольный список действий, которые необходимо выполнить для обновления существующего приложения до EF6.</span><span class="sxs-lookup"><span data-stu-id="9580e-110">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="9580e-111">1. Установка пакета NuGet EF6</span><span class="sxs-lookup"><span data-stu-id="9580e-111">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="9580e-112">Необходимо выполнить обновление до новой среды выполнения Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9580e-112">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="9580e-113">Щелкните проект правой кнопкой мыши и выберите пункт **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="9580e-113">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="9580e-114">На вкладке "в **сети** " выберите **EntityFramework** и нажмите кнопку " **установить** ".</span><span class="sxs-lookup"><span data-stu-id="9580e-114">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="9580e-115">Если была установлена предыдущая версия пакета NuGet EntityFramework, обновит ее до EF6.</span><span class="sxs-lookup"><span data-stu-id="9580e-115">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="9580e-116">Кроме того, можно выполнить следующую команду из консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="9580e-116">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="9580e-117">2. Убедитесь, что удалены ссылки на System.Data.Entity.dll сборки</span><span class="sxs-lookup"><span data-stu-id="9580e-117">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="9580e-118">Установка пакета NuGet EF6 должна автоматически удалить все ссылки на System. Data. Entity из вашего проекта.</span><span class="sxs-lookup"><span data-stu-id="9580e-118">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="9580e-119">3. переключайте любые модели конструктора EF (EDMX) для использования создания кода EF 6. x</span><span class="sxs-lookup"><span data-stu-id="9580e-119">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="9580e-120">При наличии моделей, созданных с помощью конструктора EF, необходимо обновить шаблоны создания кода, чтобы создать совместимый с EF6 код.</span><span class="sxs-lookup"><span data-stu-id="9580e-120">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="9580e-121">В настоящее время в Visual Studio 2012 и 2013 доступны только шаблоны генераторов EF 6. x DbContext.</span><span class="sxs-lookup"><span data-stu-id="9580e-121">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="9580e-122">Удаление существующих шаблонов создания кода.</span><span class="sxs-lookup"><span data-stu-id="9580e-122">Delete existing code-generation templates.</span></span> <span data-ttu-id="9580e-123">Обычно эти файлы имеют имя \*\* \<edmx_file_name\> . TT\*\* и \*\* \<edmx_file_name\> . Context.tt\*\* и будьте вложены в файл EDMX в Обозреватель решений.</span><span class="sxs-lookup"><span data-stu-id="9580e-123">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="9580e-124">Можно выбрать шаблоны в обозреватель решений и нажать клавишу **Del** , чтобы удалить их.</span><span class="sxs-lookup"><span data-stu-id="9580e-124">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="9580e-125">В проектах веб-сайтов шаблоны не будут вложены в файл EDMX, но перечислены вместе с ним в обозреватель решений.</span><span class="sxs-lookup"><span data-stu-id="9580e-125">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="9580e-126">В проектах VB.NET необходимо включить параметр "Показать все файлы", чтобы иметь возможность просматривать файлы вложенных шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9580e-126">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="9580e-127">Добавьте соответствующий шаблон создания кода EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="9580e-127">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="9580e-128">Откройте модель в конструкторе EF, щелкните правой кнопкой мыши область конструктора и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="9580e-128">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="9580e-129">Если вы используете API DbContext (рекомендуется), то на вкладке " **данные** " будет доступен **генератор DbContext EF 6. x** .</span><span class="sxs-lookup"><span data-stu-id="9580e-129">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="9580e-130">При использовании Visual Studio 2012 необходимо установить инструменты EF 6 для этого шаблона.</span><span class="sxs-lookup"><span data-stu-id="9580e-130">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="9580e-131">Дополнительные сведения см. в разделе [получение Entity Framework](xref:ef6/fundamentals/install) .</span><span class="sxs-lookup"><span data-stu-id="9580e-131">See [Get Entity Framework](xref:ef6/fundamentals/install) for details.</span></span>  

    - <span data-ttu-id="9580e-132">Если вы используете интерфейс ObjectContext API, необходимо выбрать вкладку "в **сети** " и найти **генератор EntityObject EF 6. x**.</span><span class="sxs-lookup"><span data-stu-id="9580e-132">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="9580e-133">Если вы применили какие-либо настройки к шаблонам создания кода, потребуется повторно применить их к обновленным шаблонам.</span><span class="sxs-lookup"><span data-stu-id="9580e-133">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="9580e-134">4. Обновите пространства имен для всех используемых основных типов EF.</span><span class="sxs-lookup"><span data-stu-id="9580e-134">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="9580e-135">Пространства имен для типов DbContext и Code First не изменились.</span><span class="sxs-lookup"><span data-stu-id="9580e-135">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="9580e-136">Это означает, что для многих приложений, использующих EF 4,1 или более поздней версии, не нужно ничего менять.</span><span class="sxs-lookup"><span data-stu-id="9580e-136">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="9580e-137">Такие типы, как ObjectContext, ранее находились в System.Data.Entity.dll, были перемещены в новые пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9580e-137">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="9580e-138">Это означает, что может потребоваться обновить директивы *using* или *Import* для создания EF6.</span><span class="sxs-lookup"><span data-stu-id="9580e-138">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="9580e-139">Общее правило изменения пространства имен заключается в том, что любой тип в System. Data. \* перемещается в System. Data. Entity. Core. \*.</span><span class="sxs-lookup"><span data-stu-id="9580e-139">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="9580e-140">Иными словами, просто вставьте **Entity. Core.**</span><span class="sxs-lookup"><span data-stu-id="9580e-140">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="9580e-141">После System. Data.</span><span class="sxs-lookup"><span data-stu-id="9580e-141">after System.Data.</span></span> <span data-ttu-id="9580e-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="9580e-142">For example:</span></span>

- <span data-ttu-id="9580e-143">System. Data. Ентитексцептион => System. Data. **Entity. Core**. ентитексцептион</span><span class="sxs-lookup"><span data-stu-id="9580e-143">System.Data.EntityException => System.Data.**Entity.Core**.EntityException</span></span>  
- <span data-ttu-id="9580e-144">System. Data. objects. ObjectContext => System. Data. **Entity. Core**. Objects. ObjectContext</span><span class="sxs-lookup"><span data-stu-id="9580e-144">System.Data.Objects.ObjectContext => System.Data.**Entity.Core**.Objects.ObjectContext</span></span>  
- <span data-ttu-id="9580e-145">Класс System. Data. objects. RelationshipManager => System. Data. **Entity. Core**. Objects. coclass. RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="9580e-145">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core**.Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="9580e-146">Эти типы находятся в *основных* пространствах имен, так как они не используются напрямую для большинства приложений на основе DbContext.</span><span class="sxs-lookup"><span data-stu-id="9580e-146">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="9580e-147">Некоторые типы, которые были частью System.Data.Entity.dll, по-прежнему используются как обычно, так и напрямую для приложений на основе DbContext и поэтому не были перемещены в *основные* пространства имен.</span><span class="sxs-lookup"><span data-stu-id="9580e-147">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="9580e-148">А именно:</span><span class="sxs-lookup"><span data-stu-id="9580e-148">These are:</span></span>

- <span data-ttu-id="9580e-149">System. Data. EntityState => System. Data. **Сущность**. EntityState</span><span class="sxs-lookup"><span data-stu-id="9580e-149">System.Data.EntityState => System.Data.**Entity**.EntityState</span></span>  
- <span data-ttu-id="9580e-150">Класс System. Data. objects. Едмфунктионаттрибуте => System. Data. **Entity. дбфунктионаттрибуте**</span><span class="sxs-lookup"><span data-stu-id="9580e-150">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="9580e-151">Этот класс был переименован; класс с прежним именем по-прежнему существует и работает, но теперь помечен как устаревший.</span><span class="sxs-lookup"><span data-stu-id="9580e-151">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="9580e-152">System. Data. objects. Ентитифунктионс => System. Data. **Entity. дбфунктионс**</span><span class="sxs-lookup"><span data-stu-id="9580e-152">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="9580e-153">Этот класс был переименован; класс с прежним именем по-прежнему существует и работает, но теперь помечен как устаревший.)</span><span class="sxs-lookup"><span data-stu-id="9580e-153">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="9580e-154">Пространственные классы (например, заданное DbGeography, заданное DbGeometry) были перемещены из System. Data. пространственные => System. Data. **Сущность**. Привод</span><span class="sxs-lookup"><span data-stu-id="9580e-154">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity**.Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="9580e-155">Некоторые типы в пространстве имен System. Data находятся в System.Data.dll, который не является сборкой EF.</span><span class="sxs-lookup"><span data-stu-id="9580e-155">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="9580e-156">Эти типы не были перемещены, поэтому их пространства имен остаются неизменными.</span><span class="sxs-lookup"><span data-stu-id="9580e-156">These types have not moved and so their namespaces remain unchanged.</span></span>
