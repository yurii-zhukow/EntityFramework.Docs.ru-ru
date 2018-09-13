---
title: Обновление до платформы Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 2e2dacfe67238bdb7fd1f31f784319049f0f2cb0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490952"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="65901-102">Обновление до платформы Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="65901-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="65901-103">В предыдущих версиях EF код был разделить между основными библиотеками (в основном System.Data.Entity.dll), в состав платформы .NET Framework и библиотеки (OOB)-каналу (в основном EntityFramework.dll) поставляется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="65901-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="65901-104">EF6 принимает код из библиотеки ядра и включает его в библиотеках OOB.</span><span class="sxs-lookup"><span data-stu-id="65901-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="65901-105">Это потребовалось, чтобы разрешить EF сделать открытым исходным кодом и для того, чтобы иметь возможность развиваться в разных темпе с .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="65901-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="65901-106">Вследствие этого является то, что приложения должны быть перестроено на перемещенные типы.</span><span class="sxs-lookup"><span data-stu-id="65901-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="65901-107">Это должно быть простым для приложений, которые используют DbContext как поставляемый в EF 4.1 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="65901-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="65901-108">Немного больше работы требуется для приложения, использующие класса ObjectContext, но он по-прежнему не трудно.</span><span class="sxs-lookup"><span data-stu-id="65901-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="65901-109">Ниже приведен контрольный список задач, которые необходимо выполнить для обновления существующего приложения в EF6.</span><span class="sxs-lookup"><span data-stu-id="65901-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="65901-110">1. Установите пакет EF6 NuGet</span><span class="sxs-lookup"><span data-stu-id="65901-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="65901-111">Необходимо обновить до новой среды выполнения Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="65901-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="65901-112">Щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="65901-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="65901-113">В разделе **Online** вкладке выберите **EntityFramework** и нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="65901-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="65901-114">Если предыдущая версия EntityFramework NuGet, пакет установлен это обновит ее к EF6.</span><span class="sxs-lookup"><span data-stu-id="65901-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="65901-115">Также можно запустить следующую команду из консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="65901-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="65901-116">2. Убедитесь, что удалены ссылки на сборки для System.Data.Entity.dll</span><span class="sxs-lookup"><span data-stu-id="65901-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="65901-117">Установка пакета EF6 NuGet автоматически следует удалить все ссылки на System.Data.Entity из проекта для вас.</span><span class="sxs-lookup"><span data-stu-id="65901-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="65901-118">3. Замените любые модели EF конструктора (EDMX), создания кода EF 6.x</span><span class="sxs-lookup"><span data-stu-id="65901-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="65901-119">При наличии модели, созданные в конструкторе EF, будет необходимо обновить шаблоны создания кода, чтобы создать совместимый код EF6.</span><span class="sxs-lookup"><span data-stu-id="65901-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="65901-120">Доступны в настоящее время только EF 6.x DbContext генератор шаблонов для Visual Studio 2012 и 2013.</span><span class="sxs-lookup"><span data-stu-id="65901-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="65901-121">Удалите существующие шаблоны создания кода.</span><span class="sxs-lookup"><span data-stu-id="65901-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="65901-122">Эти файлы обычно называется  **\<edmx_file_name\>.tt** и  **\<edmx_file_name\>. Context.tt** и быть вложен в узел файла edmx в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="65901-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="65901-123">Можно выбрать шаблоны, в обозревателе решений и нажмите клавишу **Del** ключа для их удаления.</span><span class="sxs-lookup"><span data-stu-id="65901-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="65901-124">В проектах веб-сайт шаблоны будут не вложен в файле edmx, но в списке с его в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="65901-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="65901-125">В проектах VB.NET необходимо включить «Показать все файлы» иметь возможность см. в файлах вложенный шаблон.</span><span class="sxs-lookup"><span data-stu-id="65901-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="65901-126">Добавьте соответствующий шаблон генерации кода EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="65901-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="65901-127">Откройте модель в конструкторе EF, щелкните правой кнопкой мыши область конструктора и выберите **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="65901-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="65901-128">Если вы используете API DbContext (рекомендуется). затем **EF 6.x генератор DbContext** будут доступны в разделе **данных** вкладки.</span><span class="sxs-lookup"><span data-stu-id="65901-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="65901-129">Если вы используете Visual Studio 2012, необходимо установить инструменты EF 6, чтобы этот шаблон.</span><span class="sxs-lookup"><span data-stu-id="65901-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="65901-130">См. в разделе [получение Entity Framework](~/ef6/fundamentals/install.md) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="65901-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="65901-131">Если вы используете ObjectContext API, а затем будет необходимо выбрать **Online** вкладку и выполните поиск **EF 6.x EntityObject Generator**.</span><span class="sxs-lookup"><span data-stu-id="65901-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="65901-132">Если вы применили все настройки, шаблоны создания кода, вам потребуется повторно применить их к в обновленных шаблонах.</span><span class="sxs-lookup"><span data-stu-id="65901-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="65901-133">4. Обновление пространства имен для использования типов EF core</span><span class="sxs-lookup"><span data-stu-id="65901-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="65901-134">Пространства имен для типов DbContext и Code First не изменились.</span><span class="sxs-lookup"><span data-stu-id="65901-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="65901-135">Это означает, что для многих приложений, использующих EF 4.1 или более поздней версии не нужно ничего менять.</span><span class="sxs-lookup"><span data-stu-id="65901-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="65901-136">Типы как ObjectContext, которые ранее находились в System.Data.Entity.dll были перемещены в новые пространства имен.</span><span class="sxs-lookup"><span data-stu-id="65901-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="65901-137">Это означает, что может потребоваться обновить ваш *с помощью* или *импорта* директивы для сборки с EF6.</span><span class="sxs-lookup"><span data-stu-id="65901-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="65901-138">Общее правило для изменения пространства имен является то, что любой тип в System.Data.\* перемещается в System.Data.Entity.Core.\*.</span><span class="sxs-lookup"><span data-stu-id="65901-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="65901-139">Другими словами, просто вставьте **Entity.Core.**</span><span class="sxs-lookup"><span data-stu-id="65901-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="65901-140">После System.Data.</span><span class="sxs-lookup"><span data-stu-id="65901-140">after System.Data.</span></span> <span data-ttu-id="65901-141">Пример:</span><span class="sxs-lookup"><span data-stu-id="65901-141">For example:</span></span>

- <span data-ttu-id="65901-142">System.Data.EntityException = > System.Data. **Entity.Core.** EntityException</span><span class="sxs-lookup"><span data-stu-id="65901-142">System.Data.EntityException => System.Data.**Entity.Core.** EntityException</span></span>  
- <span data-ttu-id="65901-143">System.Data.Objects.ObjectContext = > System.Data. **Entity.Core.** Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="65901-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core.** Objects.ObjectContext</span></span>  
- <span data-ttu-id="65901-144">System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core.** Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="65901-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core.** Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="65901-145">Эти типы находятся в *Core* пространства имен, так как они не используются непосредственно для большинства приложений на основе DbContext.</span><span class="sxs-lookup"><span data-stu-id="65901-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="65901-146">Некоторые типы, которые были частью System.Data.Entity.dll по-прежнему используются часто, так и непосредственно для приложений на основе DbContext и поэтому не были перемещены в *Core* пространства имен.</span><span class="sxs-lookup"><span data-stu-id="65901-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="65901-147">Эти особые значения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="65901-147">These are:</span></span>

- <span data-ttu-id="65901-148">System.Data.EntityState = > System.Data. **Сущности.** EntityState</span><span class="sxs-lookup"><span data-stu-id="65901-148">System.Data.EntityState => System.Data.**Entity.** EntityState</span></span>  
- <span data-ttu-id="65901-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="65901-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="65901-150">Этот класс был переименован; класс со старым именем все еще существует и работает, но он теперь помечен как устаревший.</span><span class="sxs-lookup"><span data-stu-id="65901-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="65901-151">System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="65901-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="65901-152">Этот класс был переименован; класс со старым именем все еще существует и работает, но теперь помечены как устаревшие.)</span><span class="sxs-lookup"><span data-stu-id="65901-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="65901-153">Пространственные классов (например, DbGeography, DbGeometry) перешли из System.Data.Spatial = > System.Data. **Сущности.** Пространственных</span><span class="sxs-lookup"><span data-stu-id="65901-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity.** Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="65901-154">Некоторые типы в пространстве имен System.Data находятся в System.Data.dll, которая не является сборкой EF.</span><span class="sxs-lookup"><span data-stu-id="65901-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="65901-155">Эти типы не были перемещены и поэтому их пространства имен остаются неизменными.</span><span class="sxs-lookup"><span data-stu-id="65901-155">These types have not moved and so their namespaces remain unchanged.</span></span>
