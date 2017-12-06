---
title: "Обновление с EF Core RC2 1.0 до RTM - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="4ac08-102">Обновление до RTM EF Core 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="4ac08-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="4ac08-103">Эта статья содержит руководство по переносу в приложении, построенном с пакетами RC2 для 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="4ac08-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="4ac08-104">Версии пакетов</span><span class="sxs-lookup"><span data-stu-id="4ac08-104">Package Versions</span></span>

<span data-ttu-id="4ac08-105">Имена пакетов верхнего уровня, которые вы обычно устанавливаются в приложение, не изменился между RC2 и RTM.</span><span class="sxs-lookup"><span data-stu-id="4ac08-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="4ac08-106">**Необходимо обновить до RTM-версии установленных пакетов:**</span><span class="sxs-lookup"><span data-stu-id="4ac08-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="4ac08-107">Среда выполнения пакетов (например `Microsoft.EntityFrameworkCore.SqlServer`) изменен с `1.0.0-rc2-final` для `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="4ac08-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="4ac08-108">`Microsoft.EntityFrameworkCore.Tools` Пакета изменен с `1.0.0-preview1-final` для `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="4ac08-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="4ac08-109">Обратите внимание, что инструментарий по-прежнему предварительного выпуска.</span><span class="sxs-lookup"><span data-stu-id="4ac08-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="4ac08-110">Существующие миграции может понадобиться добавить maxLength</span><span class="sxs-lookup"><span data-stu-id="4ac08-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="4ac08-111">В версии-кандидата 2, определение столбца при миграции выглядела `table.Column<string>(nullable: true)` и длина столбца было искать в некоторые метаданные хранятся в коде для миграции.</span><span class="sxs-lookup"><span data-stu-id="4ac08-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="4ac08-112">В окончательной первоначальной версии, длина теперь включены в создании кода `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="4ac08-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="4ac08-113">Любой существующий миграций, которые были формирования шаблонов перед использованием RTM не будет иметь `maxLength` указан аргумент.</span><span class="sxs-lookup"><span data-stu-id="4ac08-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="4ac08-114">Это означает, что будет использоваться максимальную длину, поддерживаемую базой данных (`nvarchar(max)` на сервере SQL Server).</span><span class="sxs-lookup"><span data-stu-id="4ac08-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="4ac08-115">Это может быть нормально для некоторых столбцов, но столбцы, являющиеся частью ключа, внешнего ключа или индекса должны быть обновлены для включения максимальную длину.</span><span class="sxs-lookup"><span data-stu-id="4ac08-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="4ac08-116">По соглашению 450 имеет максимальную длину для ключей, внешние ключи и индексированных столбцов.</span><span class="sxs-lookup"><span data-stu-id="4ac08-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="4ac08-117">Если длина настроены явным образом в модели, затем вам следует использовать заданной длины.</span><span class="sxs-lookup"><span data-stu-id="4ac08-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="4ac08-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="4ac08-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="4ac08-119">Это изменение влияет на проекты, использующие ASP.NET Identity и были созданы из предварительной-RTM шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="4ac08-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="4ac08-120">Шаблон проекта включает миграции, используемый для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="4ac08-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="4ac08-121">Этот вид миграции необходимо отредактировать для указания максимальной длиной `256` для следующих столбцов.</span><span class="sxs-lookup"><span data-stu-id="4ac08-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="4ac08-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="4ac08-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="4ac08-123">Имя</span><span class="sxs-lookup"><span data-stu-id="4ac08-123">Name</span></span>

    * <span data-ttu-id="4ac08-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="4ac08-124">NormalizedName</span></span>

*  <span data-ttu-id="4ac08-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="4ac08-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="4ac08-126">Адрес эл. почты</span><span class="sxs-lookup"><span data-stu-id="4ac08-126">Email</span></span>

   * <span data-ttu-id="4ac08-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="4ac08-127">NormalizedEmail</span></span>

   * <span data-ttu-id="4ac08-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="4ac08-128">NormalizedUserName</span></span>

   * <span data-ttu-id="4ac08-129">UserName</span><span class="sxs-lookup"><span data-stu-id="4ac08-129">UserName</span></span>

<span data-ttu-id="4ac08-130">Чтобы внести это изменение приведет к следующее исключение при применении первоначальной миграции базы данных.</span><span class="sxs-lookup"><span data-stu-id="4ac08-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="4ac08-131">.NET core: Удалите «imports» в project.json</span><span class="sxs-lookup"><span data-stu-id="4ac08-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="4ac08-132">Если нацеливания .NET Core с RC2, необходимо добавить `imports` в файл project.json в качестве временного решения для некоторых EF основных зависимостей, не поддерживая .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4ac08-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="4ac08-133">Теперь можно извлечь их.</span><span class="sxs-lookup"><span data-stu-id="4ac08-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="4ac08-134">UWP: Добавление переадресации привязок</span><span class="sxs-lookup"><span data-stu-id="4ac08-134">UWP: Add binding redirects</span></span>

<span data-ttu-id="4ac08-135">При попытке выполнения команд EF на универсальной платформы Windows (UWP) проектов приводит к возникновению следующей ошибки:</span><span class="sxs-lookup"><span data-stu-id="4ac08-135">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="4ac08-136">Необходимо вручную добавить перенаправления привязки в проекте UWP.</span><span class="sxs-lookup"><span data-stu-id="4ac08-136">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="4ac08-137">Создайте файл с именем `App.config` в проекте корневой папки и добавления перенаправления для правильные версии сборок.</span><span class="sxs-lookup"><span data-stu-id="4ac08-137">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
