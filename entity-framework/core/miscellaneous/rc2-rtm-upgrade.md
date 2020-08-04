---
title: Обновление с EF Core 1,0 RC2 до RTM — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: f496806ea6330c60cf43068882b7de839e18e383
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526775"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="b10d5-102">Обновление с EF Core 1,0 от RC2 до RTM</span><span class="sxs-lookup"><span data-stu-id="b10d5-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="b10d5-103">В этой статье приводятся рекомендации по перемещению приложения, созданного с помощью пакетов RC2, в 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="b10d5-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="b10d5-104">Версии пакета</span><span class="sxs-lookup"><span data-stu-id="b10d5-104">Package Versions</span></span>

<span data-ttu-id="b10d5-105">Имена пакетов верхнего уровня, которые обычно устанавливаются в приложение, не изменились между RC2 и RTM.</span><span class="sxs-lookup"><span data-stu-id="b10d5-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="b10d5-106">**Необходимо обновить установленные пакеты до версий RTM:**</span><span class="sxs-lookup"><span data-stu-id="b10d5-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="b10d5-107">Пакеты среды выполнения (например, `Microsoft.EntityFrameworkCore.SqlServer` ) изменились с `1.0.0-rc2-final` на `1.0.0` .</span><span class="sxs-lookup"><span data-stu-id="b10d5-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="b10d5-108">`Microsoft.EntityFrameworkCore.Tools`Пакет изменен с `1.0.0-preview1-final` на `1.0.0-preview2-final` .</span><span class="sxs-lookup"><span data-stu-id="b10d5-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="b10d5-109">Обратите внимание, что инструментарий по-прежнему является предварительным выпуском.</span><span class="sxs-lookup"><span data-stu-id="b10d5-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="b10d5-110">Для существующих миграций может потребоваться добавить maxLength</span><span class="sxs-lookup"><span data-stu-id="b10d5-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="b10d5-111">В RC2 определение столбца в процессе миграции выглядит так, как `table.Column<string>(nullable: true)` и длина столбца была просмотрена в некоторых метаданных, хранимых в коде для миграции.</span><span class="sxs-lookup"><span data-stu-id="b10d5-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="b10d5-112">В RTM длина теперь включена в сформированный код `table.Column<string>(maxLength: 450, nullable: true)` .</span><span class="sxs-lookup"><span data-stu-id="b10d5-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="b10d5-113">Для всех существующих миграций, сформированных до использования RTM, не будет `maxLength` указан аргумент.</span><span class="sxs-lookup"><span data-stu-id="b10d5-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="b10d5-114">Это означает, что будет использоваться максимальная длина, поддерживаемая базой данных ( `nvarchar(max)` на SQL Server).</span><span class="sxs-lookup"><span data-stu-id="b10d5-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="b10d5-115">Это может быть удобно для некоторых столбцов, но столбцы, которые являются частью ключа, внешнего ключа или индекса, должны быть обновлены для включения максимальной длины.</span><span class="sxs-lookup"><span data-stu-id="b10d5-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="b10d5-116">По соглашению 450 — это максимальная длина, используемая для ключей, внешних ключей и индексированных столбцов.</span><span class="sxs-lookup"><span data-stu-id="b10d5-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="b10d5-117">Если вы явно настроили длину в модели, вместо нее следует использовать эту длину.</span><span class="sxs-lookup"><span data-stu-id="b10d5-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="b10d5-118">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b10d5-118">ASP.NET Identity</span></span>

<span data-ttu-id="b10d5-119">Это изменение влияет на проекты, использующие ASP.NET Identity и созданные из шаблона проекта предварительной версии RTM.</span><span class="sxs-lookup"><span data-stu-id="b10d5-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="b10d5-120">Шаблон проекта включает миграцию, используемую для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="b10d5-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="b10d5-121">Чтобы указать максимальную длину для следующих столбцов, необходимо изменить эту миграцию `256` .</span><span class="sxs-lookup"><span data-stu-id="b10d5-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

* <span data-ttu-id="b10d5-122">**аспнетролес**</span><span class="sxs-lookup"><span data-stu-id="b10d5-122">**AspNetRoles**</span></span>
  * <span data-ttu-id="b10d5-123">name</span><span class="sxs-lookup"><span data-stu-id="b10d5-123">Name</span></span>
  * <span data-ttu-id="b10d5-124">нормализеднаме</span><span class="sxs-lookup"><span data-stu-id="b10d5-124">NormalizedName</span></span>
* <span data-ttu-id="b10d5-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="b10d5-125">**AspNetUsers**</span></span>
  * <span data-ttu-id="b10d5-126">Адрес электронной почты</span><span class="sxs-lookup"><span data-stu-id="b10d5-126">Email</span></span>
  * <span data-ttu-id="b10d5-127">нормализедемаил</span><span class="sxs-lookup"><span data-stu-id="b10d5-127">NormalizedEmail</span></span>
  * <span data-ttu-id="b10d5-128">нормализедусернаме</span><span class="sxs-lookup"><span data-stu-id="b10d5-128">NormalizedUserName</span></span>
  * <span data-ttu-id="b10d5-129">UserName</span><span class="sxs-lookup"><span data-stu-id="b10d5-129">UserName</span></span>

<span data-ttu-id="b10d5-130">Если не выполнить это изменение, при первоначальной миграции к базе данных будет получено следующее исключение.</span><span class="sxs-lookup"><span data-stu-id="b10d5-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="b10d5-131">.NET Core: удаление "Imports" в project.js</span><span class="sxs-lookup"><span data-stu-id="b10d5-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="b10d5-132">Если вы намерены ориентироваться на .NET Core с RC2, вам нужно было добавить `imports` project.jsв качестве временного решения для некоторых зависимостей EF Core, не поддерживающих .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="b10d5-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="b10d5-133">Теперь их можно удалить.</span><span class="sxs-lookup"><span data-stu-id="b10d5-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="b10d5-134">Начиная с версии 1,0 RTM, [пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` и не разрабатывает приложения .NET Core с помощью Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="b10d5-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="b10d5-135">Корпорация Майкрософт рекомендует [выполнить миграцию project.json в формат csproj](/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="b10d5-135">We recommend you [migrate from project.json to csproj](/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="b10d5-136">При использовании Visual Studio рекомендуется выполнить обновление до [Visual studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b10d5-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="b10d5-137">UWP: Добавление перенаправлений привязок</span><span class="sxs-lookup"><span data-stu-id="b10d5-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="b10d5-138">Попытка выполнить команды EF в проектах универсальная платформа Windows (UWP) приводит к следующей ошибке:</span><span class="sxs-lookup"><span data-stu-id="b10d5-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="b10d5-139">Необходимо вручную добавить перенаправления привязок в проект UWP.</span><span class="sxs-lookup"><span data-stu-id="b10d5-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="b10d5-140">Создайте файл с именем `App.config` в корневой папке проекта и добавьте перенаправления к правильным версиям сборки.</span><span class="sxs-lookup"><span data-stu-id="b10d5-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

```xml
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
